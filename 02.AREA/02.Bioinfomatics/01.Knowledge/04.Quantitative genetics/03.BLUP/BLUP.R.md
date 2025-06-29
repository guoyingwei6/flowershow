---
created: 2024-08-14 01:58
updated: 2025-06-10 15:00
dg-publish:
---

```R
ABLUP <- function(pedfile, datafile, vars = 5, vare = 55) {
  # Read pedigree and data files
  peddat <- read.csv(file = pedfile, header = TRUE)
  wkdat <- read.csv(file = datafile, header = TRUE)
  
  # Compute the A matrix
  A <- Amatrix(peddat, ploidy = 2)
  
  # Compute the inverse of the additive genetic matrix
  IG <- (vare / vars) * solve(A)
  
  # Create model matrices
  sex <- as.factor(wkdat$sex)
  X <- model.matrix(~ sex - 1)
  
  sire <- as.factor(wkdat$sire)
  Z <- model.matrix(~ sire - 1)
  
  # Phenotype data
  y <- wkdat$yobs
  
  # Set up MME components
  XX <- t(X) %*% X
  
  XZ <- t(X) %*% Z
  
  ZZ <- t(Z) %*% Z
  
  LHS1 <- cbind(XX, XZ)
  LHS2 <- cbind(t(XZ), (ZZ + IG))
  LHS <- rbind(LHS1, LHS2)
  
  RHS1 <- t(X) %*% y
  RHS2 <- t(Z) %*% y
  RHS <- rbind(RHS1, RHS2)
  
  # Solve the mixed model equations
  sol <- solve(LHS) %*% RHS
  return(sol)
}


RRBLUP <- function(data, trtNam, trnIdx, tstIdx, snp.start, snp.end,
                   mean2pq=3.54,vare = 245.0,vara = 35.24) {
  # hyper-parameters and additional index variables
  lambda <- mean2pq * vare / vara
  
  # Read and preprocess data
  wkdat <- read.csv(file = data, header = TRUE)
  yidx <- grep(trtNam, colnames(wkdat))
  nInd <- nrow(wkdat)
  
  # Incidence vector for the overall mean as the only fixed effect
  X <- matrix(1, nrow = nInd, ncol = 1)
  
  # SNP genotypes
  Z <- as.matrix(wkdat[, snp.start:snp.end])
  p <- apply(Z, 2, mean) / 2
  nSNP <- length(p)
  
  for (j in 1:ncol(Z)) {
    Z[,j] <- Z[,j] - 2 * p[j]
  }
  
  # adjusted phenotypes or breeding varlues
  y <- wkdat[, yidx]
  
  # Set up MME
  XX <- t(X[trnIdx,]) %*% X[trnIdx,]
  XZ <- t(X[trnIdx,]) %*% Z[trnIdx,]
  ZZ <- t(Z[trnIdx,]) %*% Z[trnIdx,]
  Xy <- t(X[trnIdx]) %*% y[trnIdx]
  Zy <- t(Z[trnIdx,]) %*% y[trnIdx]
  
  LHS1 <- cbind(XX, XZ)
  LHS2 <- cbind(t(XZ), (ZZ + diag(lambda, nrow = nSNP, ncol = nSNP)))
  LHS <- rbind(LHS1, LHS2)
  
  RHS <- rbind(Xy, Zy)
  
  # model solutions (i.e., overal mean and SNP effects)
  sol <- solve(LHS) %*% RHS
  
  # predictions
  predictions <- Z[tstIdx,] %*% sol[2:length(sol)]
  row.names(predictions)<-wkdat$animal[tstIdx]
  
  return(list(solution = round(sol, 3), predictions = round(predictions, 3)))
}

GBLUP <- function(file, snp.start, snp.end, trtNam, 
                  vare = 245.0,vara = 35.24) {
  # read-in the data
  wkdat <- read.csv(file=file, header = TRUE)
  nInd <- nrow(wkdat)
  
  # Daughter yield deviation for milk fat production
  yidx<-grep(trtNam,colnames(wkdat),ignore.case=TRUE)
  y <- wkdat[,yidx]
  
  # Genotypes and allelic frequencies 
  Z <- as.matrix(wkdat[, snp.start:snp.end])
  p <- apply(wkdat[, snp.start:snp.end], 2, mean) / 2
  sum2pq <- 2 * sum(p * (1 - p))
  nSNP <- length(p)
  
  lambda <- vare / vara
  
  # The overall mean as the only fixed effect
  X <- matrix(1, nrow = nInd, ncol = 1)
  
  # Center the SNP genotypes
  for (j in 1:ncol(Z)) {
    Z[, j] <- Z[, j] - 2 * p[j]
  }
  
  W <- diag(1, nrow = nInd, ncol = nInd)
  
  # Set up MME
  XX <- t(X) %*% X
  XW <- t(X) %*% W
  Xy <- t(X) %*% y
  Wy <- t(W) %*% y
  
  # Add 0.01 to the diagonal elements 
  G <- (Z %*% t(Z)) / sum2pq + diag(0.01, nrow = nInd, ncol = nInd)
  IG <- solve(G)
  
  LHS1 <- cbind(XX, XW)
  LHS2 <- cbind(t(XW), (diag(1, nrow = nInd, ncol = nInd) + lambda * IG))
  LHS <- rbind(LHS1, LHS2)
  
  RHS <- c(Xy[1, 1], Wy)
  
  sol <- solve(LHS) %*% RHS
  
  return(round(sol, 3))
}

```