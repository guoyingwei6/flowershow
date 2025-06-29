---
created: 2024-07-17 13:59
updated: 2025-06-10 15:00
dg-publish:
---
```R
#####################################################################################################################################################################################

########################################################++++++++++++------ScRNA-seq------++++++++++++################################################################################

#####################################################################################################################################################################################

  

################################### 01.install R packages ###################################

  

install.packages("Seurat")

install.packages("outliers")

install.packages("pbmcapply")

install.packages("doFuture")

if (!requireNamespace("BiocManager", quietly = TRUE))

install.packages("BiocManager")

BiocManager::install("singscore")

BiocManager::install("GSVA")

BiocManager::install("GSEABase")

BiocManager::install("limma")

  

install.packages("devtools")

library(devtools)

install.packages("installr")

install.packages("stringr")

library(stringr)

library(installr)

install.Rtools()

devtools::install_github('dviraran/SingleR')   # SingleR官方安装代码

install.packages('githubinstall')              # 上面不行就先安装‘githubinstall’

BiocManager::install("SingleR")       # 再不行用这个，SingleR安装极为麻烦，对网络要求极高

                          # check with library

  

  

################################### 02.下机数据质控 ###################################

  

#step1 引用R包

library(limma)

library(Seurat)

library(dplyr)

library(magrittr)

library(patchwork)

#step2 设置工作路径

setwd("E:")             #设置工作路径

  

#step3-1 取均值处理（非常重要：只有我们的数据是GeneMatrix,即行为细胞样，列为基因的矩阵时才需要这一步。）

rt=read.table("文件名",sep="\t",header=T,check.names=F)    #读取文件，并对重复基因取均值 #按\t分格 #有表头 #对它的名字不检查

  

rt=as.matrix(rt)                                           #将数据转换为矩阵

rownames(rt)=rt[,1]

exp=rt[,2:ncol(rt)]

dimnames=list(rownames(exp),colnames(exp))

data=matrix(as.numeric(as.matrix(exp)),nrow=nrow(exp),dimnames=dimnames)

data=avereps(data)                  

                                                          #如果一个基因存在于多行，就会对基因取均值

#step3-2 Read10X（如果我们的数据不是GeneMatrix，而是"matrix.mtx.gz"+"features.tsv.gz"+"barcodes.tsv.gz"时需要这一步）

#（注：barcodes.tsv.gz ---包含细胞的id；features.tsv.gz ---和基因相关的注释；matrix.mtx.gz---某个细胞里基因的一个值）

LD_MuSCs.data <- Read10X(data.dir = "E:")

dir("E:")   

  

#注：rt=read.table和Read10X是针对不同测序平台的输入代码

  

   #step4 将矩阵转换为Seurat对象，并对数据进行过滤

LD_MuSCs <- CreateSeuratObject(counts = LD_MuSCs.data, project = "LD_MuSCs", min.cells = 3, min.features = 500,)  #如果数据类型是GeneMAtrix格式，还要加上 names.delim = "_",

#创建一个对象#数据即为读取的数据 #所创建对象的名字 #最小的细胞数目 #细胞测得基因数小于500，则删掉 #样品名分隔符，按下划线分割

LD_MuSCs

  

   #step5 PercentageFeatureSet函数计算线粒体基因的百分比

LD_MuSCs[["percent.mt"]] <- PercentageFeatureSet(object = LD_MuSCs, pattern = "^MT-")  #The [[ operator can add columns to object metadata. This is a great place to stash QC stats

pdf(file="02.step5.featureViolin.pdf",width=10,height=6)           #保存基因特征小提琴图

VlnPlot(object = LD_MuSCs, features = c("nFeature_RNA", "nCount_RNA", "percent.mt"), ncol = 3)

dev.off() #关闭

  

   #step6 对数据进行过滤

LD_MuSCs <- subset(LD_MuSCs, subset = nFeature_RNA > 800 & nFeature_RNA < 4000 & percent.mt < 0.02)  #保留基因数目大于X的细胞 #线粒体基因百分率大于X%的细胞删掉

# step5和step6出现问题就敲0

LD_MuSCs

  

   #step7 测序深度的相关性绘图

pdf(file="02.step7.featureCor.pdf",width=10,height=6)                        #保存基因特征相关性图

plot1 <- FeatureScatter(object = LD_MuSCs, feature1 = "nCount_RNA", feature2 = "percent.mt",pt.size=1)

plot2 <- FeatureScatter(object = LD_MuSCs, feature1 = "nCount_RNA", feature2 = "nFeature_RNA",pt.size=1)

CombinePlots(plots = list(plot1, plot2))

dev.off()  #关闭

  

   #step8 对数据进行标准化

LD_MuSCs <- NormalizeData(object = LD_MuSCs, normalization.method = "LogNormalize", scale.factor = 10000)   #对对象进行标准化 #标准化的方法为取log #把测序深度矫正为100000（测序深度：测序得到的碱基总量（bp）与基因组大小（Genome）的比值，它是评价测序量的指标之一） 

   #step9 提取那些在细胞间变异系数较大的基因

LD_MuSCs <- FindVariableFeatures(object = LD_MuSCs, selection.method = "vst", nfeatures = 2000)             #找出对变异较大的基因 #找前1000个

  

   #step10 输出特征方差图

top10 <- head(x = VariableFeatures(object = LD_MuSCs), 10)            #把前十个变异最大的基因名字标上

pdf(file="02.step10.featureVar.pdf",width=10,height=6)                #保存基因特征方差图

plot1 <- VariableFeaturePlot(object = LD_MuSCs)                       #把基因在样品间波动的图形绘制出来

plot2 <- LabelPoints(plot = plot1, points = top10, repel = TRUE)  #除了把基因在样品间的波动绘制出来，还要进行一个标注

CombinePlots(plots = list(plot1, plot2))                         #把两个图形放在一起

dev.off()

  

  

  

################################### 03.01 数据降维-PCA主成分分析 ###################################

  

#step1 引用R包

library(limma)

library(Seurat)

library(dplyr)

library(magrittr)

#step2 设置工作目录

setwd("")                      #如果之前做了，就不用做了

  

#step3 PCA分析

all.genes <- rownames(LD_MuSCs)

LD_MuSCs <- ScaleData(LD_MuSCs, features = all.genes)   

LD_MuSCs <- ScaleData(LD_MuSCs, vars.to.regress = "percent.mt")                #PCA降维之前的标准预处理步骤

LD_MuSCs <- RunPCA(LD_MuSCs, features = VariableFeatures(object = LD_MuSCs))   #确认目标 #确认维度（即基因的数量） #把前面选出来的波动较大的基因作主成分分析

  

#step4 绘制每个PCA成分的相关基因图

pdf(file="03.01.step4.pcaGene.pdf",width=10,height=8)

VizDimLoadings(object = LD_MuSCs, dims = 1:4, reduction = "pca",nfeatures = 20)       #确认目标 #绘制前4个主成分图 #降维方法 #呈现前多少个基因

dev.off()

  

#step5 绘制主成分分析图形

pdf(file="03.01.step5.PCA.pdf",width=6.5,height=6)

DimPlot(object = LD_MuSCs, reduction = "pca")

dev.off()

  

#step6 绘制主成分分析热图

pdf(file="03.01.step6.pcaHeatmap.pdf",width=10,height=8)

DimHeatmap(object = LD_MuSCs, dims = 1:15, cells = 500, balanced = TRUE, nfeatures = 30,) #画前4个PC #包含500个细胞 #每一行放2个热图 #

dev.off()

  

#step7 绘制每个PC的p值分布和均匀分布

LD_MuSCs <- JackStraw(object = LD_MuSCs, num.replicate = 100)       #数据模拟100次

LD_MuSCs <- ScoreJackStraw(object = LD_MuSCs, dims = 1:20)

pdf(file="03.01.step7.pcaJackStraw.pdf",width=8,height=6)

JackStrawPlot(object = LD_MuSCs, dims = 1:20)

dev.off()

  

#step8 绘制肘图

pdf(file="03.01.step8.ElbowPlot.pdf",width=8,height=6)

ElbowPlot(LD_MuSCs)

dev.off()

  

  

  

################################### 03.02 数据降维-TSNE/UMAP聚类分析和marker基因 ###################################

  

  #???? Look at cluster IDs of the first 5 cells

head(Idents(LD_MuSCs), 5)

  

#step1-1 TSNE聚类

pcSelect=15                                                            #选择前20个主成分分析

LD_MuSCs <- FindNeighbors(object = LD_MuSCs, dims = 1:pcSelect)   #80           #计算邻接距离

LD_MuSCs <- FindClusters(object = LD_MuSCs, resolution = 0.5)          #81  #对细胞分组,优化标准模块化

LD_MuSCs <- RunTSNE(object = LD_MuSCs, dims = 1:pcSelect)              #83       #TSNE聚类

pdf(file="03.02.step1.TSNE.pdf",width=6.5,height=6)

TSNEPlot(object = LD_MuSCs, pt.size = 1, label = TRUE)   #TSNE可视化  pt.size = 2 #点的大小

dev.off()

  

#step1-2 UMAP聚类

pcSelect=15 

LD_MuSCs <- FindNeighbors(object = LD_MuSCs, dims = 1:pcSelect)   #80           #计算邻接距离

LD_MuSCs <- FindClusters(object = LD_MuSCs, resolution = 0.5)          #81  #对细胞分组,优化标准模块化  

LD_MuSCs <- RunUMAP(object = LD_MuSCs, dims = 1:pcSelect)

DimPlot(LD_MuSCs, reduction = "umap", label = TRUE)      

  

#You can save the object at this point so that it can easily be loaded back in without having to rerun the computationally intensive steps performed above, or easily shared with collaborators.

saveRDS(pbmc, file = "../LD_MuSCs.rds")

  

#step2

write.table(LD_MuSCs$seurat_clusters, file="03.step2.tsneCluster.txt", quote=F, sep="\t", col.names=F) #输出表格,这一步到底有什么用？？

  

#step3 寻找差异表达的特征/找到所有cluster的marker

logFCfilter=0 #改为0.25后可大大提高速度

adjPvalFilter=0.05                                                     #校正后的P值小于0.05

LD_MuSCs.markers <- FindAllMarkers(object = LD_MuSCs,

                               only.pos = FALSE,   #Only return positive markers (FALSE by default)

                               min.pct = 0, #定义最小的pct值

                               logfc.threshold = logFCfilter) #过滤

sig.markers <- LD_MuSCs.markers[(abs(as.numeric(as.vector(LD_MuSCs.markers$avg_logFC)))>logFCfilter & as.numeric(as.vector(LD_MuSCs.markers$p_val_adj))<adjPvalFilter),] #再过滤

  

  

write.table(sig.markers, file="03.step3.markers.xls", sep="\t", row.names=F, quote=F) #画marker基因的表格

  

###step3-0 find all markers of cluster 1

cluster1.markers <- FindMarkers(LD_MuSCs, ident.1 = 2, min.pct = 0)

head(cluster1.markers, n = 5)

  

###step3-1 find all markers distinguishing cluster 5 from clusters 0 and 3

cluster5.markers <- FindMarkers(LD_MuSCs, ident.1 = 5, ident.2 = c(0, 3), min.pct = 0.25)

head(cluster5.markers, n = 5)

  

#step4 绘制热图

top10 <- LD_MuSCs.markers %>% group_by(cluster) %>% top_n(n = 10, wt = avg_logFC) #绘制marker在各个cluster的热图

pdf(file="03.step4.tsneHeatmap-top10.pdf",width=12,height=9)

DoHeatmap(object = LD_MuSCs, features = top10$gene) + NoLegend()

dev.off()

  

#step5 绘制marker的小提琴图

pdf(file="03.step5.markerViolin.pdf",width=10,height=6)

VlnPlot(object = LD_MuSCs, features = c("IGLL5", "MBOAT1"))                  #选自己要研究的marker就好

dev.off()

  

#step6 绘制marker在各个cluster的散点图

pdf(file="03.step6.markerScatter.pdf", width=6.5, height=6)

FeaturePlot(object = LD_MuSCs, pt.size = 1, features = c("PTPRC"), cols = c("gray", "red"))

dev.off()

  

#step7 绘制marker在各个cluster的气泡图

pdf(file="03.step7.markerBubble.pdf",width=12,height=6)

cluster10Marker=c("MBOAT1", "NFIB", "TRPS1", "SOX4", "CNN3", "PIM2", "MZB1", "MS4A1", "ELK2AP", "IGLL5")

DotPlot(object = LD_MuSCs, features = cluster10Marker)

dev.off()

  

  

#step8 Single cell ridge plot

#details：https://github.com/satijalab/seurat/blob/master/R/visualization.R

  

  

  

  

################################### 04.01 注释细胞类型（SingleR机注）（代码有问题）###################################

  

#step1 引用R包

library(SingleR)

  

#step2 准备输入文件

counts <- LD_MuSCs@assays$RNA@counts

clusters <-LD_MuSCs@meta.data$seurat_clusters

ann=pbmc@meta.data$orig.ident #注释文件

  

#step3 注释细胞

singler = CreateSinglerObject(counts, annot = ann, "pbmc", min.genes = 0, #创建注释包的对象 #基因的表达 #细胞来源于哪个样品 ##已经过滤不需要过滤

  species = "Human", citation = "",

  ref.list = list(), normalize.gene.length = F, variable.genes = "de",

  fine.tune = F, do.signatures = T, clusters = clusters, do.main.types = T, #是否精细计算 

  reduce.file.size = T, numCores = 1) #选择Cpu核心的个数

singler$seurat = pbmc

singler$meta.data$xy = pbmc@reductions$tsne@cell.embeddings

clusterAnn=singler$singler[[2]]$SingleR.clusters.main$labels #保存结果文件

write.table(clusterAnn,file="07.clusterAnn.txt",quote=F,sep="\t",col.names=F)

write.table(singler$other,file="07.cellAnn.txt",quote=F,sep="\t",col.names=F) #输出文件

  

  

################################### 04.02 注释细胞类型（手注）###################################

  

  

  

  

  

  

  

  

  

  

  

  

  

  

################################### 05 拟时分析（部分） ###################################

  

#准备monocle分析需要的文件

monocle.matrix=as.matrix(pbmc@assays$RNA@data)

monocle.matrix=cbind(id=row.names(monocle.matrix),monocle.matrix)

write.table(monocle.matrix,file="07.monocleMatrix.txt",quote=F,sep="\t",row.names=F)

monocle.sample=as.matrix(pbmc@meta.data)

monocle.sample=cbind(id=row.names(monocle.sample),monocle.sample)

write.table(monocle.sample,file="07.monocleSample.txt",quote=F,sep="\t",row.names=F)

monocle.geneAnn=data.frame(gene_short_name = row.names(monocle.matrix), row.names = row.names(monocle.matrix))

monocle.geneAnn=cbind(id=row.names(monocle.geneAnn),monocle.geneAnn)

write.table(monocle.geneAnn,file="07.monocleGene.txt",quote=F,sep="\t",row.names=F)

write.table(singler$other,file="07.monocleClusterAnn.txt",quote=F,sep="\t",col.names=F)

write.table(sig.markers,file="07.monocleMarkers.txt",sep="\t",row.names=F,quote=F)

  

  

  

  

  

  

###################################07.GO和KEGG正在研究中###################################
```