---
created: 2024-08-14 01:25
updated: 2025-06-10 09:55
dg-publish: false
---

[**LaTeX**](https://www.latex-project.org/) 是一种高质量的排版格式，可以生成复杂的表格与数学公式，是当前电子与数学出版行业的事实标准。本文以 [**Pandoc**](https://www.pandoc.org/) 作为 LaTex 渲染引擎（一款用于标记语言文档转换的命令行工具），分门别类的总结了撰写数学公式所需要经常使用到的 LaTeX 语法，方便写作相关科技类文章时随手查阅。

## 保留字符

LaTeX 环境中具有特殊含义的保留字符，不能直接使用，必须通过指定的语法实现：

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|#|`\#`|**6**|{|`\{`|
|**2**|%|`\%`|**7**|}|`\}`|
|**3**|∧|`^\wedge`|**8**|∼|`\sim`|
|**4**|&|`\&`|**9**|∖|`\backslash`|
|**5**|_|`\_`||||

## 希腊字母

|序号|标准符号|LaTeX|首字母大写|LaTeX|使用 `var` 前缀|LaTeX|读音|
|---|---|---|---|---|---|---|---|
|**1**|𝛼|`\alpha`|||||/ˈælfə/|
|**2**|𝛽|`\beta`|||||/ˈbeɪtə/|
|**3**|𝛾|`\gamma`|Γ|`\Gamma`|𝛤|`\varGamma`|/ˈɡæmə/|
|**4**|𝛿|`\delta`|Δ|`\Delta`|𝛥|`\varDelta`|/ˈdɛltə/|
|**5**|𝜖|`\epsilon`|||𝜀|`\varepsilon`|/ˈɛpsɪlɒn/|
|**6**|𝜁|`\zeta`|||||/ˈzeɪtə/|
|**7**|𝜂|`\eta`|||||/ˈeɪtə/|
|**8**|𝜃|`\theta`|Θ|`\Theta`|𝜗、𝛩|`\vartheta`、`\varTheta`|/ˈθiːtə/|
|**9**|𝜄|`\iota`|||||/aɪˈoʊtə/|
|**10**|𝜅|`\kappa`|||𝜘|`\varkappa`|/ˈkæpə/|
|**11**|𝜆|`\lambda`|Λ|`\Lambda`|𝛬|`\varLambda`|/ˈlæmdə/|
|**12**|𝜇|`\mu`|||||/mjuː/|
|**13**|𝜈|`\nu`|||||/njuː/|
|**14**|𝜉|`\xi`|Ξ|`\Xi`|𝛯|`\varXi`|/zaɪ, ksaɪ/|
|**15**|𝑜|`o`|𝑂|`O`|||/ˈɒmɪkrɒn/|
|**16**|𝜋|`\pi`|Π|`\Pi`|𝜛、𝛱|`\varpi`、`\varPi`|/paɪ/|
|**17**|𝜌|`\rho`|||𝜚|`\varrho`|/roʊ/|
|**18**|𝜎|`\sigma`|Σ|`\Sigma`|𝜍、𝛴|`\varsigma`、`\varSigma`|/ˈsɪɡmə/|
|**19**|𝜏|`\tau`|||||/taʊ, tɔː/|
|**20**|𝜐|`\upsilon`|Υ|`\Upsilon`|𝛶|`\varUpsilon`|/ˈʌpsɪlɒn/|
|**21**|𝜙|`\phi`|Φ|`\Phi`|𝜑、𝛷|`\varphi`、`\varPhi`|/faɪ/|
|**22**|𝜒|`\chi`|||||/kaɪ/|
|**23**|𝜓|`\psi`|Ψ|`\Psi`|𝛹|`\varPsi`|/psaɪ/|
|**24**|𝜔|`\omega`|Ω|`\Omega`|𝛺|`\varOmega`|/oʊˈmeɪɡə/|
|**25**|ϝ|`\digamma`|||||/daɪ'gæmə/|

## 希伯来字母

|序号|符号|LaTeX|英文|序号|符号|LaTeX|英文|
|---|---|---|---|---|---|---|---|
|**1**|ℵ|`\aleph`|aleph|**3**|ℷ|`\gimel`|gimel|
|**2**|ℶ|`\beth`|beth|**4**|ℸ|`\daleth`|daleth|

## 二元运算符

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|+|`+`|**20**|∙|`\bullet`|
|**2**|−|`-`|**21**|⊕|`\oplus`|
|**3**|×|`\times`|**22**|⊖|`\ominus`|
|**4**|÷|`\div`|**23**|⊙|`\odot`|
|**5**|±|`\pm`|**24**|⊘|`\oslash`|
|**6**|∓|`\mp`|**25**|⊗|`\otimes`|
|**7**|◃|`\triangleleft`|**26**|◯|`\bigcirc`|
|**8**|▹|`\triangleright`|**27**|⋄|`\diamond`|
|**9**|⋅|`\cdot`|**28**|⊎|`\uplus`|
|**10**|∖|`\setminus`|**29**|△|`\bigtriangleup`|
|**11**|⋆|`\star`|**30**|▽|`\bigtriangledown`|
|**12**|∗|`\ast`|**31**|⊲|`\lhd`|
|**13**|∪|`\cup`|**32**|⊳|`\rhd`|
|**14**|∩|`\cap`|**33**|⊴|`\unlhd`|
|**15**|⊔|`\sqcup`|**34**|⊵|`\unrhd`|
|**16**|⊓|`\sqcap`|**35**|⨿|`\amalg`|
|**17**|∨|`\vee`|**36**|≀|`\wr`|
|**18**|∧|`\wedge`|**37**|†|`\dagger`|
|**19**|∘|`\circ`|**38**|‡|`\ddagger`|

## 二元关系符

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|=|`=`|**49**|⪈|`\gneq`|
|**2**|≠|`\ne`|**50**|≧|`\geqq`|
|**3**|≠|`\neq`|**51**|≱|`\ngeq`|
|**4**|≡|`\equiv`|**52**|≱|`\ngeqq`|
|**5**|≢|`\not\equiv`|**53**|≩|`\gneqq`|
|**6**|≐|`\doteq`|**54**|≩|`\gvertneqq`|
|**7**|≑|`\doteqdot`|**55**|≶|`\lessgtr`|
|**8**|=def|`\overset{\underset{\mathrm{def}}{}}{=}`|**56**|⋚|`\lesseqgtr`|
|**9**|:=|`:=`|**57**|⪋|`\lesseqqgtr`|
|**10**|∼|`\sim`|**58**|≷|`\gtrless`|
|**11**|≁|`\nsim`|**59**|⋛|`\gtreqless`|
|**12**|∽|`\backsim`|**60**|⪌|`\gtreqqless`|
|**13**|∼|`\thicksim`|**61**|⩽|`\leqslant`|
|**14**|≃|`\simeq`|**62**|⪇|`\nleqslant`|
|**15**|⋍|`\backsimeq`|**63**|⪕|`\eqslantless`|
|**16**|≂|`\eqsim`|**64**|⩾|`\geqslant`|
|**17**|≅|`\cong`|**65**|⪈|`\ngeqslant`|
|**18**|≇|`\ncong`|**66**|⪖|`\eqslantgtr`|
|**19**|≈|`\approx`|**67**|≲|`\lesssim`|
|**20**|≈|`\thickapprox`|**68**|⋦|`\lnsim`|
|**21**|≊|`\approxeq`|**69**|⪅|`\lessapprox`|
|**22**|≍|`\asymp`|**70**|⪉|`\lnapprox`|
|**23**|∝|`\propto`|**71**|≳|`\gtrsim`|
|**24**|∝|`\varpropto`|**72**|⋧|`\gnsim`|
|**25**|<|`<`|**73**|⪆|`\gtrapprox`|
|**26**|≮|`\nless`|**74**|⪊|`\gnapprox`|
|**27**|≪|`\ll`|**75**|≺|`\prec`|
|**28**|≪̸|`\not\ll`|**76**|⊀|`\nprec`|
|**29**|⋘|`\lll`|**77**|⪯|`\preceq`|
|**30**|⋘̸|`\not\lll`|**78**|⋠|`\npreceq`|
|**31**|⋖|`\lessdot`|**79**|⪵|`\precneqq`|
|**32**|>|`>`|**80**|≻|`\succ`|
|**33**|≯|`\ngtr`|**81**|⊁|`\nsucc`|
|**34**|≫|`\gg`|**82**|⪰|`\succeq`|
|**35**|≫̸|`\not\gg`|**83**|⋡|`\nsucceq`|
|**36**|⋙|`\ggg`|**84**|⪶|`\succneqq`|
|**37**|⋙̸|`\not\ggg`|**85**|≼|`\preccurlyeq`|
|**38**|⋗|`\gtrdot`|**86**|⋞|`\curlyeqprec`|
|**39**|≤|`\le`|**87**|≽|`\succcurlyeq`|
|**40**|≤|`\leq`|**88**|⋟|`\curlyeqsucc`|
|**41**|⪇|`\lneq`|**89**|≾|`\precsim`|
|**42**|≦|`\leqq`|**90**|⋨|`\precnsim`|
|**43**|≰|`\nleq`|**91**|⪷|`\precapprox`|
|**44**|≰|`\nleqq`|**92**|⪹|`\precnapprox`|
|**45**|≨|`\lneqq`|**93**|≿|`\succsim`|
|**46**|≨|`\lvertneqq`|**94**|⋩|`\succnsim`|
|**47**|≥|`\ge`|**95**|⪸|`\succapprox`|
|**48**|≥|`\geq`|**96**|⪺|`\succnapprox`|

## 几何符号

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|∥|`\parallel`|**14**|◊|`\lozenge`|
|**2**|∦|`\nparallel`|**15**|⧫|`\blacklozenge`|
|**3**|∥|`\shortparallel`|**16**|★|`\bigstar`|
|**4**|∦|`\nshortparallel`|**17**|◯|`\bigcirc`|
|**5**|⊥|`\perp`|**18**|△|`\triangle`|
|**6**|∠|`\angle`|**19**|△|`\bigtriangleup`|
|**7**|∢|`\sphericalangle`|**20**|▽|`\bigtriangledown`|
|**8**|∡|`\measuredangle`|**21**|△|`\vartriangle`|
|**9**|45∘|`45^\circ`|**22**|▽|`\triangledown`|
|**10**|◻|`\Box`|**23**|▴|`\blacktriangle`|
|**11**|◼|`\blacksquare`|**24**|▾|`\blacktriangledown`|
|**12**|⋄|`\diamond`|**25**|◂|`\blacktriangleleft`|
|**13**|◊◊|`\Diamond \lozenge`|**26**|▸|`\blacktriangleright`|

## 逻辑符号

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|∀|`\forall`|**20**|¬|`\neg`|
|**2**|∃|`\exists`|**21**|R̸|`\not\operatorname{R}`|
|**3**|∄|`\nexists`|**22**|⊥|`\bot`|
|**4**|∴|`\therefore`|**23**|⊤|`\top`|
|**5**|∵|`\because`|**24**|⊢|`\vdash`|
|**6**|&|`\And`|**25**|⊣|`\dashv`|
|**7**|∨|`\lor`|**26**|⊨|`\vDash`|
|**8**|∨|`\vee`|**27**|⊩|`\Vdash`|
|**9**|⋎|`\curlyvee`|**28**|⊨|`\models`|
|**10**|⋁|`\bigvee`|**29**|⊪|`\Vvdash`|
|**11**|∧|`\land`|**30**|⊬|`\nvdash`|
|**12**|∧|`\wedge`|**31**|⊮|`\nVdash`|
|**13**|⋏|`\curlywedge`|**32**|⊭|`\nvDash`|
|**14**|⋀|`\bigwedge`|**33**|⊯|`\nVDash`|
|**15**|𝑞¯|`\bar{q}`|**34**|⌜|`\ulcorner`|
|**16**|𝑎𝑏𝑐¯|`\bar{abc}`|**35**|⌝|`\urcorner`|
|**17**|𝑞―|`\overline{q}`|**36**|⌞|`\llcorner`|
|**18**|𝑎𝑏𝑐―|`\overline{abc}`|**37**|⌟|`\lrcorner`|
|**19**|¬|`\lnot`||||

## 集合符号

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|{}|`\{ \}`|**23**|⊏|`\sqsubset`|
|**2**|∅|`\emptyset`|**24**|⊃|`\supset`|
|**3**|∅|`\varnothing`|**25**|⋑|`\Supset`|
|**4**|∈|`\in`|**26**|⊐|`\sqsupset`|
|**5**|∉|`\notin`|**27**|⊆|`\subseteq`|
|**6**|∋|`\ni`|**28**|⊈|`\nsubseteq`|
|**7**|∩|`\cap`|**29**|⊊|`\subsetneq`|
|**8**|⋒|`\Cap`|**30**|⊊|`\varsubsetneq`|
|**9**|⊓|`\sqcap`|**31**|⊑|`\sqsubseteq`|
|**10**|⋂|`\bigcap`|**32**|⊇|`\supseteq`|
|**11**|∪|`\cup`|**33**|⊉|`\nsupseteq`|
|**12**|⋓|`\Cup`|**34**|⊋|`\supsetneq`|
|**13**|⊔|`\sqcup`|**35**|⊋|`\varsupsetneq`|
|**14**|⋃|`\bigcup`|**36**|⊒|`\sqsupseteq`|
|**15**|⨆|`\bigsqcup`|**37**|⫅|`\subseteqq`|
|**16**|⊎|`\uplus`|**38**|⊈|`\nsubseteqq`|
|**17**|⨄|`\biguplus`|**39**|⫋|`\subsetneqq`|
|**18**|∖|`\setminus`|**40**|⫋|`\varsubsetneqq`|
|**19**|∖|`\smallsetminus`|**41**|⫆|`\supseteqq`|
|**20**|×|`\times`|**42**|⊉|`\nsupseteqq`|
|**21**|⊂|`\subset`|**43**|⫌|`\supsetneqq`|
|**22**|⋐|`\Subset`|**44**|⫌|`\varsupsetneqq`|

## 箭头符号

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|⇛|`\Rrightarrow`|**36**|⟼|`\longmapsto`|
|**2**|⇚|`\Lleftarrow`|**37**|⇀|`\rightharpoonup`|
|**3**|⇒|`\Rightarrow`|**38**|⇁|`\rightharpoondown`|
|**4**|⇏|`\nRightarrow`|**39**|↼|`\leftharpoonup`|
|**5**|⟹|`\Longrightarrow`|**40**|↽|`\leftharpoondown`|
|**6**|⟹|`\implies`|**41**|↿|`\upharpoonleft`|
|**7**|⇐|`\Leftarrow`|**42**|↾|`\upharpoonright`|
|**8**|⇍|`\nLeftarrow`|**43**|⇃|`\downharpoonleft`|
|**9**|⟸|`\Longleftarrow`|**44**|⇂|`\downharpoonright`|
|**10**|⇔|`\Leftrightarrow`|**45**|⇌|`\rightleftharpoons`|
|**11**|⇎|`\nLeftrightarrow`|**46**|⇋|`\leftrightharpoons`|
|**12**|⟺|`\Longleftrightarrow`|**47**|↶|`\curvearrowleft`|
|**13**|⟺|`\iff`|**48**|↺|`\circlearrowleft`|
|**14**|⇑|`\Uparrow`|**49**|↰|`\Lsh`|
|**15**|⇓|`\Downarrow`|**50**|⇈|`\upuparrows`|
|**16**|⇕|`\Updownarrow`|**51**|⇉|`\rightrightarrows`|
|**17**|→|`\rightarrow`|**52**|⇄|`\rightleftarrows`|
|**18**|→|`\to`|**53**|↣|`\rightarrowtail`|
|**19**|↛|`\nrightarrow`|**54**|↬|`\looparrowright`|
|**20**|⟶|`\longrightarrow`|**55**|↷|`\curvearrowright`|
|**21**|←|`\leftarrow`|**56**|↻|`\circlearrowright`|
|**22**|←|`\gets`|**57**|↱|`\Rsh`|
|**23**|↚|`\nleftarrow`|**58**|⇊|`\downdownarrows`|
|**24**|⟵|`\longleftarrow`|**59**|⇇|`\leftleftarrows`|
|**25**|↔|`\leftrightarrow`|**60**|⇆|`\leftrightarrows`|
|**26**|↮|`\nleftrightarrow`|**61**|↢|`\leftarrowtail`|
|**27**|⟷|`\longleftrightarrow`|**62**|↫|`\looparrowleft`|
|**28**|↑|`\uparrow`|**63**|↪|`\hookrightarrow`|
|**29**|↓|`\downarrow`|**64**|↩|`\hookleftarrow`|
|**30**|↕|`\updownarrow`|**65**|⊸|`\multimap`|
|**31**|↗|`\nearrow`|**66**|↭|`\leftrightsquigarrow`|
|**32**|↙|`\swarrow`|**67**|⇝|`\rightsquigarrow`|
|**33**|↖|`\nwarrow`|**68**|↠|`\twoheadrightarrow`|
|**34**|↘|`\searrow`|**69**|↞|`\twoheadleftarrow`|
|**35**|↦|`\mapsto`||||

## 特殊符号

|序号|符号|LaTeX|序号|符号|LaTeX|
|---|---|---|---|---|---|
|**1**|∞|`\infty`|**33**|♭|`\flat`|
|**2**|ℵ|`\aleph`|**34**|♮|`\natural`|
|**3**|∁|`\complement`|**35**|♯|`\sharp`|
|**4**|∍|`\backepsilon`|**36**|╱|`\diagup`|
|**5**|ð|`\eth`|**37**|╲|`\diagdown`|
|**6**|Ⅎ|`\Finv`|**38**|⋅|`\centerdot`|
|**7**|ℏ|`\hbar`|**39**|⋉|`\ltimes`|
|**8**|ℑ|`\Im`|**40**|⋊|`\rtimes`|
|**9**|𝚤|`\imath`|**41**|⋋|`\leftthreetimes`|
|**10**|𝚥|`\jmath`|**42**|⋌|`\rightthreetimes`|
|**11**|𝕜𝑘|`\Bbbk`|**43**|≖|`\eqcirc`|
|**12**|ℓ|`\ell`|**44**|≗|`\circeq`|
|**13**|℧|`\mho`|**45**|≜|`\triangleq`|
|**14**|℘|`\wp`|**46**|≏|`\bumpeq`|
|**15**|ℜ|`\Re`|**47**|≎|`\Bumpeq`|
|**16**|Ⓢ|`\circledS`|**48**|≑|`\doteqdot`|
|**17**|⨿|`\amalg`|**49**|≓|`\risingdotseq`|
|**18**|%|`\%`|**50**|≒|`\fallingdotseq`|
|**19**|†|`\dagger`|**51**|⊺|`\intercal`|
|**20**|‡|`\ddagger`|**52**|⊼|`\barwedge`|
|**21**|…|`\ldots`|**53**|⊻|`\veebar`|
|**22**|⋯|`\cdots`|**54**|⩞|`\doublebarwedge`|
|**23**|⌣|`\smile`|**55**|≬|`\between`|
|**24**|⌢|`\frown`|**56**|⋔|`\pitchfork`|
|**25**|≀|`\wr`|**57**|⊲|`\vartriangleleft`|
|**26**|◃|`\triangleleft`|**58**|⋪|`\ntriangleleft`|
|**27**|▹|`\triangleright`|**59**|⊳|`\vartriangleright`|
|**28**|♢|`\diamondsuit`|**60**|⋫|`\ntriangleright`|
|**29**|♡|`\heartsuit`|**61**|⊴|`\trianglelefteq`|
|**30**|♣|`\clubsuit`|**62**|⋬|`\ntrianglelefteq`|
|**31**|♠|`\spadesuit`|**63**|⊵|`\trianglerighteq`|
|**32**|⅁|`\Game`|**64**|⋭|`\ntrianglerighteq`|

## 分数

| 类型                  | 符号                                                                         | LaTeX                                                                      |
| ------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **分数**              | $\frac{2}{4}x=0.5x or {2 \over 4}x=0.5x$                                   | `\frac{2}{4}x=0.5x or {2 \over 4}x=0.5x`                                   |
| **小型分数**            | $\tfrac{2}{4}x = 0.5x$                                                     | `\tfrac{2}{4}x = 0.5x`                                                     |
| **大型分数**  <br>（不嵌套） | $\dfrac{2}{4} = 0.5 \qquad \dfrac{2}{c + \dfrac{2}{d + \dfrac{2}{4}}} = a$ | `\dfrac{2}{4} = 0.5 \qquad \dfrac{2}{c + \dfrac{2}{d + \dfrac{2}{4}}} = a` |
| **大型分数**  <br>（嵌套）  | $\cfrac{2}{c + \cfrac{2}{d + \cfrac{2}{4}}} = a$                           | `\cfrac{2}{c + \cfrac{2}{d + \cfrac{2}{4}}} = a`                           |

## 数值函数

|符号|LaTeX|
|---|---|
|exp𝑎⁡𝑏=𝑎𝑏,exp⁡𝑏=𝑒𝑏,10𝑚|`\exp_a b = a^b, \exp b = e^b, 10^m`|
|ln⁡𝑐,lg⁡𝑑=log⁡𝑒,log10⁡𝑓|`\ln c, \lg d = \log e, \log_{10} f`|
|sin⁡𝑎,cos⁡𝑏,tan⁡𝑐,cot⁡𝑑,sec⁡𝑒,csc⁡𝑓|`\sin a, \cos b, \tan c, \cot d, \sec e, \csc f`|
|arcsin⁡𝑎,arccos⁡𝑏,arctan⁡𝑐|`\arcsin a, \arccos b, \arctan c`|
|arccot𝑑,arcsec𝑒,arccsc𝑓|`\operatorname{arccot} d, \operatorname{arcsec} e, \operatorname{arccsc} f`|
|sinh⁡𝑎,cosh⁡𝑏,tanh⁡𝑐,coth⁡𝑑|`\sinh a, \cosh b, \tanh c, \coth d`|
|sh𝑘,ch𝑙,th𝑚,coth𝑛|`\operatorname{sh}k, \operatorname{ch}l, \operatorname{th}m, \operatorname{coth}n`|
|argsh𝑜,argch𝑝,argth𝑞|`\operatorname{argsh}o, \operatorname{argch}p, \operatorname{argth}q`|
|sgn𝑟,\|𝑠\||`\operatorname{sgn}r, \left\vert s \right\vert`|
|min(𝑥,𝑦),max(𝑥,𝑦)|`\min(x,y), \max(x,y)`|

如果需要使用特殊的函数符号，那么可以采用 `\operatorname{}` 命令进行自定义：

|符号|LaTeX|
|---|---|
|mydefine𝑥|`\operatorname{mydefine}x`|

## 根式

|符号|LaTeX|符号|LaTeX|
|---|---|---|---|
|√|`\surd`|𝜋𝑛|`\sqrt[n]{\pi}`|
|𝜋|`\sqrt{\pi}`|𝑥3+𝑦323|`\sqrt[3]{\frac{x^3+y^3}{2}}*`_|

## 微分与导数

|符号|LaTeX|
|---|---|
|𝑑𝑡,d𝑡,𝜕𝑡,∇𝜓|`dt, \mathrm{d}t, \partial t, \nabla\psi`|
|𝑑𝑦/𝑑𝑥,d𝑦/d𝑥,𝑑𝑦𝑑𝑥,d𝑦d𝑥,𝜕2𝜕𝑥1𝜕𝑥2𝑦|`dy/dx, \mathrm{d}y/\mathrm{d}x, \frac{dy}{dx}, \frac{\mathrm{d}y}{\mathrm{d}x}, \frac{\partial^2}{\partial x_1\partial x_2}y`|
|′,‵,𝑓′,𝑓′,𝑓″,𝑓(3),𝑦˙,𝑦¨|`\prime, \backprime, f^\prime, f', f'', f^{(3)}, \dot y, \ddot y`|

## 模运算

|符号|LaTeX|
|---|---|
|𝑠𝑘≡0(mod𝑚)|`s_k \equiv 0 \pmod{m}`|
|𝑎mod𝑏|`a \bmod b`|
|gcd(𝑚,𝑛),lcm(𝑚,𝑛)|`\gcd(m, n), \operatorname{lcm}(m, n)`|
|∣,∤,∣,∤|`\mid, \nmid, \shortmid, \nshortmid`|

## 极限

|符号|LaTeX|
|---|---|
|lim𝑛→∞𝑥𝑛|`\lim_{n \to \infty}x_n`|
|lim𝑛→∞𝑥𝑛|`\textstyle \lim_{n \to \infty}x_n`|

## 范围与预测

|符号|LaTeX|
|---|---|
|min𝑥,max𝑦,inf𝑠,sup𝑡|`\min x, \max y, \inf s, \sup t`|
|lim𝑢,lim inf𝑣,lim sup𝑤|`\lim u, \liminf v, \limsup w`|
|dim⁡𝑝,deg⁡𝑞,det𝑚,ker⁡𝜙|`\dim p, \deg q, \det m, \ker\phi`|
|Pr𝑗,hom⁡𝑙,‖𝑧‖,arg⁡𝑧|`\Pr j, \hom l, \lVert z \rVert, \arg z`|

## 积分

|符号|LaTeX|
|---|---|
|∫13𝑒3/𝑥𝑥2𝑑𝑥|`\int\limits_{1}^{3}\frac{e^3/x}{x^2}\, dx`|
|∫13𝑒3/𝑥𝑥2𝑑𝑥|`\int_{1}^{3}\frac{e^3/x}{x^2}\, dx`|
|∫−𝑁𝑁𝑒𝑥𝑑𝑥|`\textstyle \int\limits_{-N}^{N} e^x dx`|
|∫−𝑁𝑁𝑒𝑥𝑑𝑥|`\textstyle \int_{-N}^{N} e^x dx`|
|∬𝐷𝑑𝑥𝑑𝑦|`\iint\limits_D dx\,dy`|
|∭𝐸𝑑𝑥𝑑𝑦𝑑𝑧|`\iiint\limits_E dx\,dy\,dz`|
|⨌𝐹𝑑𝑥𝑑𝑦𝑑𝑧𝑑𝑡|`\iiiint\limits_F dx\,dy\,dz\,dt`|
|∫(𝑥,𝑦)∈𝐶𝑥3𝑑𝑥+4𝑦2𝑑𝑦|`\int_{(x,y)\in C} x^3\, dx + 4y^2\, dy`|
|∮(𝑥,𝑦)∈𝐶𝑥3𝑑𝑥+4𝑦2𝑑𝑦|`\oint_{(x,y)\in C} x^3\, dx + 4y^2\, dy`|

> **注意**：积分符号采用 `\int\_{}^{}` 命令调用，双重积分符号采用 `\iint\_{}^{}`，以此类推，最高可以支持四重积分。

曲线积分可以使用 `\oint` 命令调用，但是 MathJax 并不支持该语法，因此在开启了 Unicode 扩展的前提下，可以改为采用 `\unicode{}` 命令调用：

|符号|LaTeX|描述|
|---|---|---|
|∯∯∯∯𝐶|`\unicode{8751} \unicode{x222F}_C`|曲面积分符号的 Unicode 码十进制为 `8751`,十六进制为 `x222F`；|
|∰∰∰∰𝐶|`\unicode{8752} \unicode{x2230}_C`|三维曲面积分符号的 Unicode 码十进制为 `8752`,十六进制为 `x2230`；|
|∱∱∲∲∳∳∱∱𝑐∲∲𝑐∳∳𝑐|`\unicode{8753} \unicode{x2231}_c \unicode{8754} \unicode{x2232}_c \unicode{8755} \unicode{x2233}_c`|其它积分符号；|

## 大型运算符

|分类|符号|LaTeX|符号|LaTeX|
|---|---|---|---|---|
|**求和**|∑𝑎𝑏|`\sum_{a}^{b}`|∑𝑎𝑏|`\textstyle \sum_{a}^{b}`|
|**连乘积**|∏𝑎𝑏|`\prod_{a}^{b}`|∏𝑎𝑏|`\textstyle \prod_{a}^{b}`|
|**余积**|∐𝑎𝑏|`\coprod_{a}^{b}`|∐𝑎𝑏|`\textstyle \coprod_{a}^{b}`|
|**并集**|⋃𝑎𝑏|`\bigcup_{a}^{b}`|⋃𝑎𝑏|`\textstyle \bigcup_{a}^{b}`|
|**交集**|⋂𝑎𝑏|`\bigcap_{a}^{b}`|⋂𝑎𝑏|`\textstyle \bigcap_{a}^{b}`|
|**析取**|⋁𝑎𝑏|`\bigvee_{a}^{b}`|⋁𝑎𝑏|`\textstyle \bigvee_{a}^{b}`|
|**合取**|⋀𝑎𝑏|`\bigwedge_{a}^{b}`|⋀𝑎𝑏|`\textstyle \bigwedge_{a}^{b}`|

## 上下标

| 类型             | 符号                                                       | 代码                                                                                                                                              |
| -------------- | -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| **上标**         | 𝑎2  <br>𝑎𝑥+3                                          | `a^2`  <br>`a^{x+3}`                                                                                                                            |
| **下标**         | 𝑎2                                                      | `a_2`                                                                                                                                           |
| **组合**         | 1030𝑎2+2  <br>𝑎𝑖,𝑗𝑏𝑓′                              | `10^{30} a^{2+2}`  <br>`a{i,j} b{f'}`                                                                                                           |
| **上下标混合**      | 𝑥23  <br>𝑥23                                           | `x_2^3`  <br>`{x_2}^3`                                                                                                                          |
| **上标的上标**      | 10108                                                    | `10^{10^{8}}`                                                                                                                                   |
| **混合标识**       | 12𝑋34𝑎𝑏  <br>12!Ω34                                   | `\sideset{1^2}{3^4}X_a^b`  <br>`{}_1^2!\Omega_3^4`                                                                                              |
| **顶标底标**       | 𝜔𝛼  <br>𝜔𝛼  <br>𝜔𝛾𝛼  <br>𝜔𝛼                     | `\overset{\alpha}{\omega}`  <br>`\underset{\alpha}{\omega}`  <br>`\overset{\alpha}{\underset{\gamma}{\omega}}`  <br>`\stackrel{\alpha}{\omega}` |
| **导数**         | 𝑥′,𝑦″,𝑓′,𝑓″ 𝑥′,𝑦′′                                 | `x', y'', f', f''` `x^\prime, y^{\prime\prime}`                                                                                                 |
| **导数点**        | 𝑥˙,𝑥¨                                                  | `\dot{x}, \ddot{x}`                                                                                                                             |
| **上下划线与向量**    | 𝑎^ 𝑏¯ 𝑐→  <br>𝑎𝑏→ 𝑐𝑑← 𝑑𝑒𝑓^  <br>𝑔ℎ𝑖― 𝑗𝑘𝑙― | `\hat a \ \bar b \ \vec c`  <br>`\overrightarrow{a b} \ \overleftarrow{c d} \ \widehat{d e f}`  <br>`\overline{g h i} \ \underline{j k l}`      |
| **弧度**         | 𝐴𝐵⌢                                                    | `\overset{\frown} {AB}`                                                                                                                         |
| **箭头**         | 𝐴←𝑛+𝜇−1𝐵→𝑇𝑛±𝑖−1𝐶                                 | `A \xleftarrow{n+\mu-1} B \xrightarrow[T]{n\pm i-1} C`                                                                                          |
| **大括号**        | 1+2+⋯+100⏞5050                                           | `\overbrace{ 1+2+\cdots+100 }^{5050}`                                                                                                           |
| **底部大括号**      | 𝑎+𝑏+⋯+𝑧⏟26                                            | `\underbrace{ a+b+\cdots+z }_{26}`                                                                                                              |
| **求和运算**       | ∑𝑘=1𝑁𝑘2                                               | `\sum_{k=1}^N k^2`                                                                                                                              |
| **文本模式下的求和运算** | ∑𝑘=1𝑁𝑘2                                               | `\textstyle \sum_{k=1}^N k^2`                                                                                                                   |
| **分式中的求和运算**   | ∑𝑘=1𝑁𝑘2𝑎                                             | `\frac{\sum_{k=1}^N k^2}{a}`                                                                                                                    |
| **分式中的求和运算**   | ∑𝑘=1𝑁𝑘2𝑎                                             | `\frac{\displaystyle \sum_{k=1}^N k^2}{a}`                                                                                                      |
| **分式中的求和运算**   | ∑𝑘=1𝑁𝑘2𝑎                                             | `\frac{\sum\limits^{^N}_{k=1} k^2}{a}`                                                                                                          |
| **乘积运算**       | ∏𝑖=1𝑁𝑥𝑖                                              | `\prod_{i=1}^N x_i`                                                                                                                             |
| **乘积运算**       | ∏𝑖=1𝑁𝑥𝑖                                              | `\textstyle \prod_{i=1}^N x_i`                                                                                                                  |
| **副乘运算**       | ∐𝑖=1𝑁𝑥𝑖                                              | `\coprod_{i=1}^N x_i`                                                                                                                           |
| **副乘运算**       | ∐𝑖=1𝑁𝑥𝑖                                              | `\textstyle \coprod_{i=1}^N x_i`                                                                                                                |
| **极限**         | lim𝑛→∞𝑥𝑛                                              | `\lim_{n \to \infty}x_n`                                                                                                                        |
| **极限**         | lim𝑛→∞𝑥𝑛                                              | `\textstyle \lim_{n \to \infty}x_n`                                                                                                             |
| **积分**         | ∫13𝑒3/𝑥𝑥2𝑑𝑥                                         | `\int\limits_{1}^{3}\frac{e^3/x}{x^2}\, dx`                                                                                                     |
| **积分**         | ∫13𝑒3/𝑥𝑥2𝑑𝑥                                         | `\int_{1}^{3}\frac{e^3/x}{x^2}\, dx`                                                                                                            |
| **积分**         | ∫−𝑁𝑁𝑒𝑥𝑑𝑥                                           | `\textstyle \int\limits_{-N}^{N} e^x dx`                                                                                                        |
| **积分**         | ∫−𝑁𝑁𝑒𝑥𝑑𝑥                                           | `\textstyle \int_{-N}^{N} e^x dx`                                                                                                               |
| **双重积分**       | ∬𝐷𝑑𝑥𝑑𝑦                                              | `\iint\limits_D dx\,dy`                                                                                                                         |
| **三重积分**       | ∭𝐸𝑑𝑥𝑑𝑦𝑑𝑧                                          | `\iiint\limits_E dx\,dy\,dz`                                                                                                                    |
| **四重积分**       | ⨌𝐹𝑑𝑥𝑑𝑦𝑑𝑧𝑑𝑡                                      | `\iiiint\limits_F dx\,dy\,dz\,dt`                                                                                                               |
| **路径积分**       | ∫(𝑥,𝑦)∈𝐶𝑥3𝑑𝑥+4𝑦2𝑑𝑦                              | `\int_{(x,y)\in C} x^3\, dx + 4y^2\, dy`                                                                                                        |
| **环路积分**       | ∮(𝑥,𝑦)∈𝐶𝑥3𝑑𝑥+4𝑦2𝑑𝑦                              | `\oint_{(x,y)\in C} x^3\, dx + 4y^2\, dy`                                                                                                       |
| **交集**         | ⋂𝑖=1𝑛𝐸𝑖                                              | `\bigcap_{i=1}^n E_i`                                                                                                                           |
| **并集**         | $\bigcup_{i=1}^n E_i$                                    | `\bigcup_{i=1}^n E_i`                                                                                                                           |

## 二项式系数

|类型|符号|LaTeX|
|---|---|---|
|二项式系数|(𝑛𝑘)|`\binom{n}{k}`|
|小型二项式系数|(𝑛𝑘)|`\tbinom{n}{k}`|
|大型二项式系数|(𝑛𝑘)|`\dbinom{n}{k}`|

## 矩阵

| 符号                                                  | LaTeX                                                                                                                                                                             |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $$\begin{matrix}  x & y \\  z & v  \end{matrix}$$   | \begin{matrix}  <br>x & y \\  <br>z & v  <br>\end{matrix}                                                                                                                         |
| $$\begin{vmatrix}  x & y \\  z & v  \end{vmatrix}$$ | \begin{vmatrix}  <br>x & y \\  <br>z & v  <br>\end{vmatrix}                                                                                                                       |
| ‖𝑥𝑦𝑧𝑣‖                                          | \|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{Vmatrix}  <br>x & y \\  <br>z & v  <br>\end{Vmatrix}\|                                                           |
| [0⋯0⋮⋱⋮0⋯0]                                         | \|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4  <br>5\|\begin{bmatrix}  <br>0 & \cdots & 0 \\  <br>\vdots & \ddots & \vdots \\  <br>0 & \cdots & 0  <br>\end{bmatrix}\| |
| {𝑥𝑦𝑧𝑣}                                          | \|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{Bmatrix}  <br>x & y \\  <br>z & v  <br>\end{Bmatrix}\|                                                           |
| (𝑥𝑦𝑧𝑣)                                          | \|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{pmatrix}  <br>x & y \\  <br>z & v  <br>\end{pmatrix}\|                                                           |
| (𝑎𝑏𝑐𝑑)                                          | \|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\bigl( \begin{smallmatrix}  <br>a&b\\  <br>c&d  <br>\end{smallmatrix} \bigr)\|                                          |

## 数组

|符号|LaTeX|
|---|---|
|𝑎𝑏𝑆001011101110|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4  <br>5  <br>6  <br>7  <br>8\|\begin{array}{ \\| c \\| c \\| c \\| }  <br>a & b & S \\  <br>\hline  <br>0 & 0 & 1 \\  <br>0 & 1 & 1 \\  <br>1 & 0 & 1 \\  <br>1 & 1 & 0  <br>\end{array}\||

## 方程与方程组

| 符号                                 | LaTeX                                                                                                  |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------ |
| {3𝑥+5𝑦+𝑧7𝑥−2𝑦+4𝑧−6𝑥+3𝑦+2𝑧 | ```<br>\begin{cases}  <br>3x + 5y + z \\  <br>7x - 2y + 4z \\  <br>-6x + 3y + 2z  <br>\end{cases}  ``` |
|                                    |                                                                                                        |

### 条件定义

|符号|LaTeX|
|---|---|
|𝑓(𝑛)={𝑛/2,if 𝑛 is even3𝑛+1,if 𝑛 is odd|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4  <br>5\|f(n) =  <br>\begin{cases}  <br>n/2, & \text{if }n\text{ is even} \\  <br>3n+1, & \text{if }n\text{ is odd}  <br>\end{cases}\||

### 多行等式

|符号|LaTeX|
|---|---|
|𝑓(𝑥)=(𝑎+𝑏)2=𝑎2+2𝑎𝑏+𝑏2|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{align}  <br>f(x) & = (a+b)^2\\  <br>& = a^2+2ab+b^2  <br>\end{align}\||
|𝑓(𝑥)=(𝑎−𝑏)2=𝑎2−2𝑎𝑏+𝑏2|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{alignat}{2}  <br>f(x) & = (a-b)^2 \\  <br>& = a^2-2ab+b^2  <br>\end{alignat}\||
|𝑧=𝑎𝑓(𝑥,𝑦,𝑧)=𝑥+𝑦+𝑧|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{array}{lcl}  <br>z & = & a \\  <br>f(x,y,z) & = & x + y + z  <br>\end{array}\||
|𝑧=𝑎𝑓(𝑥,𝑦,𝑧)=𝑥+𝑦+𝑧|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{array}{lcr}  <br>z & = & a \\  <br>f(x,y,z) & = & x + y + z  <br>\end{array}\||

### 自动编号

|描述|符号|LaTeX|
|---|---|---|
|开启 AMS 扩展包的情况下，会在部分环境的多行公式后自动编号；|𝐸=𝑚𝑐2𝑒𝑖𝜋+1=0|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{eqnarray}  <br>E = mc^2 \\  <br>e^{i\pi}+1=0  <br>\end{eqnarray}\||
|整个公式都不进行编号，可以使用`{equation*}`、`{eqnarray*}`环境；|𝐸=𝑚𝑐2𝑒𝑖𝜋+1=0|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{eqnarray*}  <br>E = mc^2 \\  <br>e^{i\pi}+1=0  <br>\end{eqnarray*}\||
|单个方程不进行编号，可以在指定方程后面添加`\nonumber`命令；|𝐸=𝑚𝑐2𝑒𝑖𝜋+1=0|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{eqnarray}  <br>E = mc^2 \\  <br>e^{i\pi}+1=0 \nonumber  <br>\end{eqnarray}\||
|个别公式出现或者不出现编号，可以在公式后使用`\tag{}`或者`\notag`命令；|𝐸=𝑚𝑐2(b)𝑒𝑖𝜋+1=0|\|   \|   \|<br>\|---\|---\|<br>\|1  <br>2  <br>3  <br>4\|\begin{eqnarray}  <br>E = mc^2 \notag\\  <br>e^{i\pi}+1=0 \tag{b}  <br>\end{eqnarray}\||

## 括号

常用的 `()`、`[]`、`{}` 括号符号可以在 LaTeX 环境当中直接进行使用，但是如果处于较大的符号当中，就应该配合 `\left` 与 `\right` 命令来使用：

|类型|符号|LaTeX|
|---|---|---|
|圆括号、小括号|(𝑎𝑏)|`\left ( \frac{a}{b} \right )`|
|方括号、中括号|[𝑎𝑏]  <br>[𝑎𝑏]|`\left [ \frac{a}{b} \right ]`  <br>`\left \lbrack \frac{a}{b} \right \rbrack`|
|花括号、大括号|{𝑎𝑏}  <br>{𝑎𝑏}|`\left \{ \frac{a}{b} \right \}`  <br>`\left \lbrace \frac{a}{b} \right \rbrace`|
|角括号|⟨𝑎𝑏⟩|`\left \langle \frac{a}{b} \right \rangle`|
|单竖线和双竖线|\|𝑎𝑏\|  <br>‖𝑐𝑑‖|`\left \vert \frac{a}{b} \right \vert`  <br>`\left \Vert \frac{c}{d} \right \Vert`|
|取整函数与取顶函数|⌊𝑎𝑏⌋  <br>⌈𝑐𝑑⌉|`\left \lfloor \frac{a}{b} \right \rfloor`  <br>`\left \lceil \frac{c}{d} \right \rceil`|
|斜线与反斜线|/𝑎𝑏\|`\left / \frac{a}{b} \right \backslash`|
|上下箭头|↑𝑎𝑏↓  <br>⇑𝑎𝑏⇓  <br>↕𝑎𝑏⇕|`\left \uparrow \frac{a}{b} \right \downarrow`  <br>`\left \Uparrow \frac{a}{b} \right \Downarrow`  <br>`\left \updownarrow \frac{a}{b} \right \Updownarrow`|
|混合括号|[0,1)  <br>⟨𝜓\||`\left [ 0,1 \right )`  <br>`\left \langle \psi \right \vert`|
|使用`\left.`和`\right.`不显示某侧的括号；|𝐴𝐵}→𝑋|`\left. \frac{A}{B} \right \} \to X`|

> **注意**：在 Markdown 当中使用 LaTeX 时，为了避免语法冲突，花括号 `{}` 必须采用 `\{`和`\}` 进行转义，或者改用 `\lbrace` 和 `\rbrace` 的方式；如果是在 Markdown 表格当中使用 LaTeX，则必须采用 `\vert` 或者 `\Vert` 代替直接在公式当中书写 `|` 和 `||`。

### 括号尺寸

|符号|LaTeX|
|---|---|
|(((((…]]]]]|`( \bigl( \Bigl( \biggl( \Biggl( \dots \Biggr] \biggr] \Bigr] \bigr] ]`|
|{{{{{…⟩⟩⟩⟩⟩|`\{ \bigl \{ \Bigl \{ \biggl \{ \Biggl \{ \dots \Biggr\rangle \biggr\rangle \Bigr\rangle \bigr\rangle \rangle`|
|\|…\||`\vert \big \vert \Big \vert \bigg \vert \Bigg \vert \dots \Bigg \vert \bigg \vert \Big \vert \big \vert`|
|⌊⌊⌊⌊⌊…⌉⌉⌉⌉⌉|`\lfloor \bigl\lfloor \Bigl\lfloor \biggl\lfloor \Biggl\lfloor \dots \Biggr\rceil \biggr\rceil \Bigr\rceil \bigr\rceil \rceil`|
|↑↑↑↑↑⋯⇓⇓⇓⇓⇓|`\uparrow \big\uparrow \Big\uparrow \bigg\uparrow \Bigg\uparrow \dots \Bigg\Downarrow \bigg\Downarrow \Big\Downarrow \big\Downarrow \Downarrow`|
|↕↕↕↕↕⋯⇕⇕⇕⇕⇕|`\updownarrow \big\updownarrow \Big\updownarrow \bigg\updownarrow \Bigg\updownarrow \dots \Bigg\Updownarrow \bigg\Updownarrow \Big\Updownarrow \big\Updownarrow \Updownarrow`|
|/////…\\\\∖|`/ \big/ \Big/ \bigg/ \Bigg/ \dots \Bigg\backslash \bigg\backslash \Big\backslash \big\backslash \backslash`|

## 空格与换行

|序号|符号|LaTeX|
|---|---|---|
|**双空格**|𝑎𝑏|`a \qquad b`|
|**单空格**|𝑎𝑏|`a \quad b`|
|**字符空格**|𝑎 𝑏|`a\ b`|
|**文本模式中的字符空格**|𝑎 𝑏|`a \text{ } b`|
|**大空格**|𝑎𝑏|`a\;b`|
|**小空格**|𝑎𝑏|`a\,b`|
|**极小空格**|𝑎𝑏|`ab`|
|**极小空格**（用于区分语法）|𝑎𝑏|`a b`|
|**无空格**（用于区分多字母变量）|ab|`\mathit{ab}`|

MathJax 3.0 取消了单行公式环境下 `\\` 的强制换行功能，因此强制换行命令 `\\` 仅能用于 `eqnarray`、`align`、`array`、`matrix` 等多行环境当中。除此之外，还可以在 `\displaylines{}` 行显示命令当中使用 `\\` 强制换行命令：

|符号|LaTeX|
|---|---|
|𝑦=1729𝑥𝑦=1729−𝑥|`\displaylines{y=1729x \\ y=1729-x}`|

总结