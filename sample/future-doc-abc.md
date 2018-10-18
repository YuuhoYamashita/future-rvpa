---
title: "RVPA��p�����Ǘ���l�EABC�v�Z�`���[�g���A��"
date: "2018-10-11"
output: 
  html_document:
   highlight: pygments
   theme: cerulean
   toc: yes
   toc_depth: 3
   toc_float: yes
---




## 0. �X�V���FHP���J�łƂ̂�����

2018/10/11

- �ȑO���炠����out.vpa(VPA���̌��ʂ�csv�t�@�C���ɏo�͂���)�ɉ����āAread.vpa(����̏�����csv�t�@�C���ɂ�����VPA���ʂ�ǂ��,future.vpa��est.MSY2�Ŏg����vpa���ʂ̃I�u�W�F�N�g�ɂ���)��ǉ�
- future.vpa��waa.fun�I�v�V�����ɂ�����o�O���C���i���ȑO�̃o�[�W�����ł́A�����\���̂P�N�ڂ�weight at age�̌v�Z�Ƀ~�X������܂����j
- ���̑��Afuture.vpa��pre.catch, new.rec�I�v�V�������g���Ă���ꍇ�Aest.MSY2�����܂������Ȃ������C��

2018/9/28

- �����\���֐���future2.1.r�ɍX�V
- �Đ��Y�֌W����̊֐���fit.HS, fit.BH, fit.RI����A�ꊇ���ē����֐��Ő��肷��fit.SR�Ɉڍs
- MSY����֐���est.MSY����est.MSY2�ցiest.MSY2�ł͋ߔN�̎��ȑ��ւ��l�������Ǘ���l���v�Z�ł��܂��j
- HCR�ɂ�������𐄒肷��֐� calc.beta��ǉ�
- ABC�v�Z�܂ł�R�R�[�h��ǉ�

## 1. ���O����
- �f�[�^�̓ǂݍ��݁CRVPA�֐��̓ǂݍ��݂Ȃ�
- �����Ŏg���֐��ƃf�[�^�ւ̃����N
<!---    - <a href="rvpa1.9.2.r" download="rvpa1.9.2.r">rvpa1.9.2.r</a>  --->
<!---    - <a href="future1.11.r" download="future1.11.r">future1.11.r</a>     --->
    - <a href="http://cse.fra.affrc.go.jp/ichimomo/fish/rvpa1.9.2.r">rvpa1.9.2.r</a>   
    - <a href="https://www.dropbox.com/s/rjpqks8zpuzeqwy/future2.1.r?dl=0">future2.1.r</a>   
    - [��f�[�^](http://cse.fra.affrc.go.jp/ichimomo/fish/data.zip) (�W�J���č�ƃt�H���_�Ƀf�[�^��u��)



```r
# �֐��̓ǂݍ��� ��  warning�܂��́u�x���v���o�邩������܂��񂪁C���̌㓮���Ă���Ζ�肠��܂���
source("../program/rvpa1.9.2.r")
source("../program/future2.1.r")

# �f�[�^�̓ǂݍ���
caa <- read.csv("caa_pma.csv",row.names=1)
waa <- read.csv("waa_pma.csv",row.names=1)
maa <- read.csv("maa_pma.csv",row.names=1)
dat <- data.handler(caa=caa, waa=waa, maa=maa, M=0.5)
names(dat)
```

```
## [1] "caa"        "maa"        "waa"        "index"      "M"         
## [6] "maa.tune"   "waa.catch"  "catch.prop"
```


## 2. VPA�ɂ�鎑���ʐ���

�����vpa�֐��̕Ԃ�l�Cres.pma���g���ď����\���v�Z�������Ȃ��Ă����̂ŁC���̂��߂�vpa�����{���܂��D(���̕ӂ͂��܂�ڂ���������܂���D)


```r
# VPA�ɂ�鎑���ʐ���
res.pma <- vpa(dat,fc.year=2009:2011,rec=585,rec.year=2011,tf.year = 2008:2010,
               term.F="max",stat.tf="mean",Pope=TRUE,tune=FALSE,p.init=1.0)
```


```r
res.pma$Fc.at.age # �����\����MSY�v�Z�Ŏg��current F (fc.year�̃I�v�V�����ł���F�̕��ς����w�肳���)
```

```
##         0         1         2         3 
## 0.5436309 1.1766833 1.3059521 1.3059519
```

```r
plot(res.pma$Fc.at.age,type="b",xlab="Age",ylab="F",ylim=c(0,max(res.pma$Fc.at.age)))
```

![plot of chunk unnamed-chunk-1](figure/unnamed-chunk-1-1.png)

## <font color="Red"> 2.5 VPA���ʂ��O������ǂݍ��ޏꍇ(2018/10/11�ǉ�) </font>

�ȉ��̂悤��read.vpa�֐����g���ĉ������B
�T���v���ƂȂ�"out.csv"��<a href="http://cse.fra.affrc.go.jp/ichimomo/fish/out.csv"> ������ </a> �ɂ���܂��B


```r
 res.pma2 <- read.vpa("out.csv")
```


## 3. �Đ��Y�֌W�����肵�Ȃ��Ǘ���l�̌v�Z
- ref.F�֐����g���܂�
- %SPR��Fmax�ȂǁA�Đ��Y�֌W�����肵�Ȃ��Ǘ���l���v�Z���܂�
- �v�Z���ʂ�rres.pma�Ɋi�[����܂�
- YPR, SPR�Ȑ���Fcurrent (```res.pma$Fc.at.a```�ɓ����Ă���l�ł�), Fmax, Fmed, F0.1�Ȃǂ̈ʒu���\������܂�


```r
byear <- 2009:2011 # �����p�����[�^�𕽋ς�����Ԃ�2009�N����2011�N�Ƃ���
rres.pma <- ref.F(res.pma, # VPA�̌v�Z����
                  waa.year=byear, maa.year=byear, M.year=byear, # weight at age, maturity at age, M��2009����2011�N�܂ł̕��ςƂ���
                  rps.year=2000:2011, # Fmed���v�Z����Ƃ��ɗp����RPS�͈̔�
                  max.age=Inf, # SPR�v�Z�ŉ��肷��N��̍ő�l 
                  pSPR=c(10,20,30,35,40), # F_%SPR���v�Z����Ƃ��ɁC���p�[�Z���g��SPR���v�Z���邩
                  Fspr.init=1)
```

![**�}�Fplot=TRUE�ŕ\�������YPR, SPR�Ȑ�**](figure/ref.F-1.png)

- ���ʂ̃T�}���[��```rres.pma$summary```�ɂ���Č���܂�
- max: F at age�̍ő�l�Cmean: F at age�̕��ϒl�CFref/Fcur: Fcurrent�𕪕�ɂ����Ƃ���F�Ǘ���l�̔�
- ���̌��ʂ���C�����F�iFcurrent�j��Fmed�Ƃقړ����iFref/Fcur=0.96�Ȃ̂Łj�CF��SRP=10�����炢�ł��邱�Ƃ��킩��܂�


```r
rres.pma$summary
```

```
##           Fcurrent      Fmed     Flow     Fhigh      Fmax      F0.1
## max       1.305952 1.2545205 1.482739 1.0427937 0.7069380 0.3956762
## mean      1.083055 1.0404012 1.229668 0.8648116 0.5862791 0.3281429
## Fref/Fcur 1.000000 0.9606176 1.135370 0.7984931 0.5413200 0.3029791
##               Fmean FpSPR.10.SPR FpSPR.20.SPR FpSPR.30.SPR FpSPR.35.SPR
## max       1.2552763     1.434605    0.8363638    0.5648693    0.4739020
## mean      1.0410280     1.189749    0.6936147    0.4684585    0.3930172
## Fref/Fcur 0.9611963     1.098512    0.6404245    0.4325345    0.3628785
##           FpSPR.40.SPR
## max          0.4000316
## mean         0.3317549
## Fref/Fcur    0.3063142
```

## 4. �Đ��Y�֌W�̐���
### �f�[�^�̍쐬

- get.SRdata���g���čĐ��Y�֌W�̃t�B�b�g�p�̃f�[�^�����
- get.SRdata�֐��ł́C```rownames(res.pma$naa)```���Q�Ƃ��A�K�v�ȔN���SSB�����炵���f�[�^���쐬����
- year�͉����N


```r
# VPA���ʂ��g���čĐ��Y�f�[�^�����
SRdata <- get.SRdata(res.pma)
head(SRdata)
```

```
## $year
##  [1] 1982 1983 1984 1985 1986 1987 1988 1989 1990 1991 1992 1993 1994 1995
## [15] 1996 1997 1998 1999 2000 2001 2002 2003 2004 2005 2006 2007 2008 2009
## [29] 2010 2011
## 
## $SSB
##  [1] 12199.02 15266.68 15072.03 19114.22 23544.42 28769.36 34764.44
##  [8] 38219.49 48535.10 61891.08 63966.56 38839.78 53404.29 47322.39
## [15] 54485.00 54385.04 47917.14 46090.77 59847.97 53370.62 48781.21
## [22] 42719.43 40095.38 39311.72 38335.98 37564.69 27604.88 23079.33
## [29] 22902.91 21427.90
## 
## $R
##  [1]  406.0086  498.9652  544.3007  469.6025 1106.8877 1043.4237  696.7049
##  [8]  923.9567 1353.1790 1698.8457 1117.5454 2381.1352 1669.1381 1818.3638
## [15] 1858.0043 1458.9524 1334.9288 1116.9433 1100.4598 1693.9770 1090.7181
## [22] 1081.8377 1265.0559 1024.0418  753.5207  764.9720  879.6573  513.1128
## [29]  570.9195  585.0000
```


```r
# SSB��R�̃f�[�^�����������Ă���ꍇ
SRdata0 <- get.SRdata(R.dat=exp(rnorm(10)),SSB.dat=exp(rnorm(10)))
# ����̊��Ԃ̃f�[�^�������g���ꍇ
SRdata0 <- get.SRdata(res.pma,years=1990:2000) 
```

### ���f���̃t�B�b�g
- HS,BH,RI���t�B�b�g���C�Đ��Y�֌W�̃p�����[�^�𐄒肷��
- ���ʂ̃I�u�W�F�N�g��AICc��AICc�̒l�������Ă���̂ŁC������r���C�Đ��Y�֌W�����肷��
- �ȑO�̃`���[�g���A���ł�fit.HS, fit.BH, fit.RI�ȂǁA���Ă͂߂�֐����ƂɊ֐��𕪂��Ă��܂������A�ꊇ����SR.fit�֐��Ōv�Z�ł���悤�ɂȂ�܂����B
- SR.fit�I�v�V����
    - SR:�Đ��Y�֌W�̃^�C�v�F "HS"�i�z�b�P�[�E�X�e�B�b�N�j�A"BH"�i�׃o�[�g���E�z���g�j�A"RI"�i���b�J�[�j
    - AR: ���ȑ��ւ̍l���Ȃ�(AR=1)�A�ߋ��P�N���̎��ȑ��ւ��l��(AR=1)
    �i�P�N�������Ή����Ă��Ȃ��j
    - method: �ŏ����@�i"L2")���ŏ���Βl�@�i"L1"�j
    - **���ȑ��ւ���E�Ȃ���AICc���r���A���ȑ��ւ���ꂽ�ق����������ǂ������f����**
        - $\log(R_t)=\log(HS(SSB_t))+\rho \times {\log(R_{t-1})-\log(HS(SSB_{t-1}))}$
        - $\log(R_t)~N(\log(R_t),\sigma^2)$
	- **���ȑ��փp�����[�^rho�̐���ɂ��Ă͕s����ȕ���������܂��B�v�Z���@�̉��P�ɂ�荡��l���ς��\��������܂�**
	- ���̗�̏ꍇ��HS��AR�Ȃ��ōł�AICc����������MSY�v�Z�ł�HS.par0�̌��ʂ��g��

```r
HS.par0 <- fit.SR(SRdata,SR="HS",method="L2",AR=0,hessian=FALSE)
HS.par1 <- fit.SR(SRdata,SR="HS",method="L2",AR=1,hessian=FALSE)
BH.par0 <- fit.SR(SRdata,SR="BH",method="L2",AR=0,hessian=FALSE)
BH.par1 <- fit.SR(SRdata,SR="BH",method="L2",AR=1,hessian=FALSE)
RI.par0 <- fit.SR(SRdata,SR="RI",method="L2",AR=0,hessian=FALSE)
RI.par1 <- fit.SR(SRdata,SR="RI",method="L2",AR=1,hessian=FALSE)
c(HS.par0$AICc,HS.par1$AICc,BH.par0$AICc,BH.par1$AICc,RI.par0$AICc,RI.par1$AICc)
```

```
## [1] 10.66778 13.27672 11.44803 14.11029 11.40670 14.05799
```
- ���ʂ̐}��

```r
plot.SRdata(SRdata)
points(HS.par0$pred$SSB,HS.par0$pred$R,col=2,type="l",lwd=3)
points(BH.par0$pred$SSB,BH.par0$pred$R,col=3,type="l",lwd=3)    
points(RI.par0$pred$SSB,RI.par0$pred$R,col=4,type="l",lwd=3)
```

![�}�F**�ϑ��l�i���j�ɑ΂���Đ��Y�֌W���Dplot=�Ԃ�HS�C�΂Ɛ�BH, RI�������҂͂قƂ�Ǐd�Ȃ��Ă��Č����Ȃ�**](figure/unnamed-chunk-4-1.png)

- TMB�I�v�V����(```TMB=TRUE```)���g���܂��i**������ƕs����ł��B�g�������ꍇ�͂��₢���킹��������**�j\
[autoregressiveSR2.cpp](http://cse.fra.affrc.go.jp/ichimomo/fish/autoregressiveSR2.cpp)���_�E�����[�h���āC��ƃt�H���_�ɒu��

```r
# install.packages("TMB")�@#TMB���C���X�g�[������ĂȂ����
library(TMB)
compile("autoregressiveSR2.cpp")
dyn.load(dynlib("autoregressiveSR2"))
HS.par11 <- fit.SR(SRdata,SR="HS",method="L2",AR=1,TMB=TRUE) #marginal likelihood
```

### ���f���f�f
�Đ��Y�֌W�̂��Ă͂߂̂��Ƃ́A���肳�ꂽ�p�����[�^�̐M����Ԃ�挒���Ȃǂ��`�F�b�N����K�v������܂��B���̂��߂̊֐��Q�Ȃǂ��p�ӂ��Ă��܂��B�ڂ�����<a href=SRR-guidline0.html> SRR�K�C�h���C�� </a> ��


## 5. �����\��

future.vpa�֐����g���܂�

- recfunc�̈����ɍĐ��Y�֌W�̊֐����Crec.arg��recfunc�ɑ΂�������i�Đ��Y�֌W�̃p�����[�^�j������
- �o�[�W�����A�b�v�ɂƂ��Ȃ��A���p�ł���Đ��Y�֌W�̊֐������Ȃ��Ȃ�܂���
     - *** HS.rec: �z�b�P�[�E�X�e�B�b�N�{�����̃��T���v�����O�i���ȑ��ւ���̏ꍇ�͑Ή������j[���̊֐��A�ꎞ�I�Ɏg���Ȃ��Ȃ��Ă��܂��I�g���K�v���邩�����A����������] ***
     - HS.recAR: �z�b�P�[�E�X�e�B�b�N�{�����͑ΐ����K���z�{���ȑ��ւ���̏ꍇ���Ή�
     - RI.recAR�EBH.recAR�FHS.recAR�̃��b�J�[�E�׃o�[�g���z���g�o�[�W����


```r
fres.HS <- future.vpa(res.pma,
                      multi=1,
                      nyear=50, # �����\���̔N��
                      start.year=2012, # �����\���̊J�n�N
                      N=100, # �m���I�v�Z�̌J��Ԃ���
                      ABC.year=2013, # ABC���v�Z����N
                      waa.year=2009:2011, # �����p�����[�^�̎Q�ƔN
                      maa.year=2009:2011,
                      M.year=2009:2011,
                      is.plot=TRUE, # ���ʂ��v���b�g���邩�ǂ���
                      seed=1,
                      silent=TRUE,
                      recfunc=HS.recAR, # �Đ��Y�֌W�̊֐�
                      # recfunc�ɑ΂������
                      rec.arg=list(a=HS.par0$pars$a,b=HS.par0$pars$b,
                                   sd=HS.par0$pars$sd,resid=HS.par0$resid))
```

![**�}�Fis.plot=TRUE�ŕ\�������}�D������(Biomass)�C�e��������(SSB), ���l��(Catch)�̎��n��D����_�I�����\���iDeterministic�j�C���ϒl�iMean�j�C�����l(Median)�C80���M����Ԃ�\��**](figure/future.vpa-1.png)

Beverton-Holt�����肷��ꍇ


```r
fres.BH <- future.vpa(res.pma,
                      multi=1,
                      nyear=50, # �����\���̔N��
                      start.year=2012, # �����\���̊J�n�N
                      N=100, # �m���I�v�Z�̌J��Ԃ���
                      ABC.year=2013, # ABC���v�Z����N
                      waa.year=2009:2011, # �����p�����[�^�̎Q�ƔN
                      maa.year=2009:2011,
                      M.year=2009:2011,
                      is.plot=TRUE, # ���ʂ��v���b�g���邩�ǂ���
                      seed=1,
                      silent=TRUE,
                      recfunc=BH.recAR, # �Đ��Y�֌W�̊֐�
                      # recfunc�ɑ΂������
                      rec.arg=list(a=BH.par0$pars$a,b=BH.par0$pars$b,
                                   sd=BH.par0$pars$sd,resid=BH.par0$resid))
```

![**�}�Fis.plot=TRUE�ŕ\�������}�D������(Biomass)�C�e��������(SSB), ���l��(Catch)�̎��n��D����_�I�����\���iDeterministic�j�C���ϒl�iMean�j�C�����l(Median)�C80���M����Ԃ�\��**](figure/future.vpa2-1.png)

�����������g���Ă�����x�����\��������

- ```fres.HS$input```�ɁA�����\���Ŏg���������������Ă���̂ŁA�����do.call(�֐��A����)����Ɠ����v�Z���J��Ԃ���

```r
fres.HS2 <- do.call(future.vpa,fres.HS$input)
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

- fres.HS$input���㏑�����邱�ƂŁC�����������g���Ȃ���ݒ�����������ύX���������\�������s�ł���
- ����```multi```��current F�ւ̏搔�ɂȂ�
- ���Ƃ���multi=1����multi=0.5�ɕύX�����͈ȉ��̂Ƃ���


```r
# ������input.tmp�ɑ���D
input.tmp <- fres.HS2$input
# �����̈ꕔ��ς���
input.tmp$multi <- 0.5 # current F��1/2�ŋ��l
fres.HS3 <- do.call(future.vpa,input.tmp)
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)

plot.futures�֐����g���ĕ����̌��ʂ��r


```r
par(mfrow=c(2,2))
plot.futures(list(fres.HS,fres.HS3),legend.text=c("F=Fcurrent","F=0.5Fcurrent"),target="SSB")
plot.futures(list(fres.HS,fres.HS3),legend.text=c("F=Fcurrent","F=0.5Fcurrent"),target="Catch")
plot.futures(list(fres.HS,fres.HS3),legend.text=c("F=Fcurrent","F=0.5Fcurrent"),target="Biomass") 
```

![�}�Fplot.futures�֐��̌���](figure/unnamed-chunk-8-1.png)

### (5-1) F�̐ݒ��Frec

�����\���ɂ����鋙�l�̃V�i���I

- future.vpa�̈���```ABC.year```�Ŏw�肵���N����CFcurrent �~ multi�ɂ��F�ŋ��l�����
- ABC.year-1�N�܂ł�Fcurrent�ɂ�鋙�l
- Frec�Ɉ�����^���邱�ƂŁC�C�ӂ̎����ʂɔC�ӂ̊m���ŉ񕜂�����悤�ȏ����\�����ł��܂��D

**Frec�̃I�v�V����**

|�I�v�V����             |����                              |
|:----------------------|:---------------------------------|
|stochastic | �m���I�����\�������Ƃ�Frec���v�Z���邩�ǂ��� |
|future.year | �����𖞂����Ă��邩�ǂ����𔻒f����N |
|Blimit | �����Ƃ��Ďg����臒l |
|scenario | ="blimit": Blimit��**�����**�m����target.probs�ɂ��� |
|         | ="catch.mean": future.year�N�̕��ϋ��l�ʂ�Blimit�̒l�ƈ�v������ |
|         | ="ssb.mean": future.year�N�̕��ϐe���ʂ�Blimit�̒l�ƈ�v������ | 
|target.probs| scenario="blimit"�̂Ƃ��ɖړI�Ƃ���m���i�p�[�Z���g�Ŏw��j|
|Frange | �T������F�͈̔́D�w�肵�Ȃ��ꍇ�Cc(0.01,multi*2)�͈̔͂ŒT�����܂��̂ŁC���܂�����ł��Ȃ��ꍇ��future.vpa�̈���multi��ς��邩�C���̃I�v�V�����ł���炵��F�̒l�Ɍ��肵�Ă�������|



```r
# ���Ƃ��Ό���̎����ʂɈێ�����V�i���I
fres.currentSSB <- future.vpa(res.pma,
                      multi=0.8,
                      nyear=50, # �����\���̔N��
                      start.year=2012, # �����\���̊J�n�N
                      N=100, # �m���I�v�Z�̌J��Ԃ���
                      ABC.year=2013, # ABC���v�Z����N
                      waa.year=2009:2011, # �����p�����[�^�̎Q�ƔN
                      maa.year=2009:2011,
                      M.year=2009:2011,seed=1,
                      is.plot=TRUE, # ���ʂ��v���b�g���邩�ǂ���
                      Frec=list(stochastic=TRUE,future.year=2023,Blimit=rev(colSums(res.pma$ssb))[1],scenario="blimit",target.probs=50),
                      recfunc=HS.recAR, # �Đ��Y�֌W�̊֐�
                      # recfunc�ɑ΂������
                      rec.arg=list(a=HS.par0$pars$a,b=HS.par0$pars$b,
                                   sd=HS.par0$pars$sd,bias.corrected=TRUE))
```

```
## F multiplier=  0.8 seed= 1 
## F multiplier= 1.05842
```

![Frec�I�v�V�������g�����ꍇ�́A���ʂ̐}�ɖړI�Ƃ���N�E�����ʂ̂Ƃ���ɐԐ�������܂��B���ꂪ�����\���̌��ʂƈ�v���Ă��邩�m���߂Ă��������B������v���Ă��Ȃ��ꍇ�Amulti�i�����l�j��Frec�̃I�v�V������Frange���w�肵�Ă�蒼���Ă�������](figure/unnamed-chunk-9-1.png)

### (5-2) �Đ��Y�֌W

- �c�����T���v�����O�ŏ����\��������ꍇ��refunc�Ƃ���HS.rec���g���i***!�ꎞ�I�Ɏg���܂���!***�j

```r
# �c�����T���v�����O�ɂ�鏫���\��
fres.HS4 <- future.vpa(res.pma,
                          multi=1,
                          nyear=50, # �����\���̔N��
                          start.year=2012, # �����\���̊J�n�N
                          N=100, # �m���I�v�Z�̌J��Ԃ���
                          ABC.year=2013, # ABC���v�Z����N
                          waa.year=2009:2011, # �����p�����[�^�̎Q�ƔN
                          maa.year=2009:2011,
                          M.year=2009:2011,
                          is.plot=TRUE, # ���ʂ��v���b�g���邩�ǂ���
                          seed=1,
                          recfunc=HS.rec, # �Đ��Y�֌W�̊֐��iHS.rec=Hockey-stick)                                
                          rec.arg=list(a=HS.par0$pars$a,b=HS.par0$pars$b,
                                       sd=HS.par0$pars$sd,bias.correction=TRUE,
                                       resample=TRUE,resid=HS.par0$resid))
```

```
## F multiplier=  1 seed= 1
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10-1.png)

�c�����T���v�����O���ΐ����K���z���̈Ⴂ���r


```r
par(mfrow=c(2,2))
plot(fres.HS$vssb[,-1],fres.HS$naa[1,,-1],xlab="SSB",ylab="Recruits") 
plot(fres.HS4$vssb[,-1],fres.HS4$naa[1,,-1],xlab="SSB",ylab="Recruits") 
plot.futures(list(fres.HS,fres.HS4)) # ���҂̔�r
```

![plot of chunk unnamed-chunk-11](figure/unnamed-chunk-11-1.png)

### (5-3) �N��ʑ̏d�����������ɉe�������ꍇ�̏����\���i2018/06/12�V�I�v�V�����Ƃ��Ēǉ��j
- ***future.vpa�ŁCwaa.fun = TRUE�Ƃ���΁A�N��ʎ����d�ʂ����������ilog(�̏d)~log(��������)�̉�A���֐������Ŏ��s�j�̊֐�����\������܂�***
- ***�s�m�������l������܂�***
- 30�n�Q�ł��Ă͂߂����<a href="waa-lm.pdf">������</a> (�f�[�^��1�N���Â��ł�)
- �����m�}�C���V�C�Δn�}�C���V�C�����m�}�T�o�C�z�b�P�C���˓��T�����ł͔N��ʑ̏d�ƔN��ʎ��������Ɋ֌W�����肻���Ȃ��񂶂ł�


```r
lm.res <- plot.waa(res.pma) # weight at age�����������̊֐��ɂȂ��Ă��邩�ǂ����C�m�F���Ă݂�D���̗�̏ꍇ�͓��ɗL�ӂȊ֌W�͂Ȃ�
```

```
## Warning in summary.lm(lm.list[[i]]): essentially perfect fit: summary may
## be unreliable

## Warning in summary.lm(lm.list[[i]]): essentially perfect fit: summary may
## be unreliable

## Warning in summary.lm(lm.list[[i]]): essentially perfect fit: summary may
## be unreliable

## Warning in summary.lm(lm.list[[i]]): essentially perfect fit: summary may
## be unreliable
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-1.png)

```r
# lm.res�̒��ɉ�A�������ʂ��N����������Ă��܂�
fres.HS6 <- fres.HS
fres.HS6$input$waa.fun <- TRUE
fres.HS6$input$N <- 1000
fres.HS6 <- do.call(future.vpa, fres.HS6$input)
```

![plot of chunk unnamed-chunk-12](figure/unnamed-chunk-12-2.png)

## 6. MSY�Ǘ���l�̌v�Z
- MSY�Ǘ���l�v�Z�ł́C��L�̏����\���ɂ����āCF�̒l��l�X�ɕς����Ƃ��̕��t��ԁi���㎞�ԁ~20�N��```nyear```�Ŏw�肵�܂��j�ɂ����鎑���ʂ₻��ɑΉ�����F�����Ǘ���l�Ƃ��ĎZ�o���܂�
- *** �Ȃ̂ŁA�����܂ł̃v���Z�X�ŁAABC�v�Z�̂��߂ɂ�����Ƃ����I�v�V������ݒ肵��future.vpa�����s���Ă����Ă��������B���̕Ԃ�l```fres.HS```��MSY�v�Z�ł͎g���Ă����܂� ***
<--- [* ```is.plot=TRUE```�Ƃ����F��l�X�ɕς����Ƃ��̕��ϐe�������ʂƕ��ϋ��l�ʁC�Ή�����F�̊Ǘ���l���o�͂��܂�] --->
- est.MSY(������ƌÂ��o�[�W�����AB0���MSY���Z�o����܂�)��est.MSY2�i�V�����o�[�W�����AAR����̏ꍇ�ɑΉ����܂��j�̂Q������܂��BABC�̎��Z�ɂ�est.MSY2���g���ĉ�����

### est.MSY(AR���l�����Ă��Ȃ��o�[�W����)�̐���
- ���̊֐��Ōv�Z�ł���Ǘ���l�͈ȉ��̂悤�Ȃ��̂ɂȂ�܂�

| �Ǘ���l |���� | 
|:----------------------|:---------------------------------|
| SSB_MSY | ���t��Ԃɂ����ĕ��ύő務�l�ʂ��ő�ɂȂ�Ƃ��̐e���� |
| SSB_0 (XX%) | F=0�ŏ����\�������Ƃ��̕��t��Ԃɂ�����e����($B_0$)�ɑ΂��銄���i����```B0percent```��c(0.4, 0.5)�̂悤�Ɏw�肵�܂��j |
| SSB_PGY (LXX%) (HXX%)| SS_MSY�ŒB������鋙�l�ʂ�XX%��B������Ƃ��̐e���ʂ̉����܂��͏���i����```PGY```��c(0.9, 0.95)�̂悤�Ɏw�肵�܂��j |



```r
# MSY�Ǘ���l�̌v�Z

# ���㎞�Ԃ̌v�Z������20�{��MSY�v�Z�̂����̏����\�����ԂɂȂ�܂�
GT <- Generation.Time(res.pma,maa.year=2009:2011, M.year=2009:2011,Plus = 100)

MSY.HS <- est.MSY(res.pma, # VPA�̌v�Z����
                 fres.HS$input, # �����\���Ŏg�p��������
                 nyear=20*GT,N=100, # �����\���̔N���C�J��Ԃ���
                 PGY=c(0.9,0.6,0.1),B0percent=c(0.3,0.4)) # PGY��B0%���x��
```

```
## Estimating MSY
## F multiplier= 0.5252164 
## Estimating PGY  90 %
## F multiplier= 0.2636767 
## F multiplier= 0.9704383 
## Estimating PGY  60 %
## F multiplier= 0.1132016 
## F multiplier= 1.043157 
## Estimating PGY  10 %
## F multiplier= 0.01321078 
## F multiplier= 1.108316 
## Estimating B0  30 %
## F multiplier= 0.4330439 
## Estimating B0  40 %
## F multiplier= 0.3064784
```

![**�}�Fest.MSY��is.plot=TRUE�Ōv�Z�������ɕ\�������}�DF�̋����ɑ΂��镽�t��Ԃ̐e�������ʁi���j�Ƌ��l�ʁi�E�j�D���肳�ꂽ�Ǘ���l���\���D**](figure/msy-1.png)

���ʂ̗v���```MSY.HS$summary```�ɂȂ�܂��D


```r
# ���ʂ̕\��
MSY.HS$summary
```

```
##                    SSB        B          U    Catch  Fref/Fcur
## MSY           125040.8 223373.5  0.3236417 72292.99  0.5252164
## B0            503044.1 613191.2          0        0          0
## PGY_0.9_upper 223824.4 327619.4  0.1985929 65062.91  0.2636767
## PGY_0.9_lower 57821.15   144908  0.4489952 65063.01  0.9704383
## PGY_0.6_upper 341598.2 448906.4 0.09661459 43370.91  0.1132016
## PGY_0.6_lower 35632.34 93349.12  0.4645684 43367.05   1.043157
## PGY_0.1_upper 478855.7 588662.5 0.01228751 7233.198 0.01321078
## PGY_0.1_lower 5643.458 15059.02  0.4794063 7219.388   1.108316
## B0-30%        150916.3 251086.1  0.2856432 71721.05  0.4330439
## B0-40%        201214.2 304061.4  0.2230259 67813.58  0.3064784
##               Fref2Fcurrent          F0         F1         F2         F3
## MSY               0.5252164   0.2855239  0.6180134  0.6859075  0.6859074
## B0                        0           0          0          0          0
## PGY_0.9_upper     0.2636767   0.1433428   0.310264  0.3443492  0.3443491
## PGY_0.9_lower     0.9704383   0.5275602   1.141898   1.267346   1.267346
## PGY_0.6_upper     0.1132016   0.0615399  0.1332025  0.1478359  0.1478359
## PGY_0.6_lower      1.043157   0.5670922   1.227465   1.362313   1.362312
## PGY_0.1_upper    0.01321078 0.007181791 0.01554491 0.01725265 0.01725265
## PGY_0.1_lower      1.108316   0.6025147   1.304136   1.447407   1.447407
## B0-30%            0.4330439   0.2354161  0.5095555  0.5655346  0.5655345
## B0-40%            0.3064784   0.1666111   0.360628  0.4002461   0.400246
```

- MSY.HS�ɂ́CF=0, F=Fmsy, F=�����Ŏw�肳�ꂽPGY��SPR�ɑΉ�����F�ŏ����\���������ʂ��i�[����Ă��܂��i���t�@�C���T�C�Y�傫���Ȃ�܂��̂Œ��ӁI�j
    - fout0: F=0�̌���
    - fout.msy: F=Fmsy�̌���
    - fout.B0percent: F=F0��ɂ��F�i�����̌��ʂ����X�g�`���œ����Ă��܂��j
    - fout.PGY: PGY��ɂ��F�i�����̌��ʂ����X�g�`���œ����Ă��܂��j

```r
names(MSY.HS)
```

```
## [1] "all.stat"       "summary"        "trace"          "fout0"         
## [5] "fout.msy"       "fout.B0percent" "fout.PGY"
```
### est.MSY2�iAR���l���ł���o�[�W�����GABC�̎��Z�ɂ͂�������g���ĉ������j�̐���

```r
par(mfrow=c(1,1))
# est.MSY2�ł͐��㎞�Ԃ̌v�Z�~�Q�O�N���������I�ɏ����\������悤�ɂȂ��Ă��܂�
MSY.HS2 <- est.MSY2(res.pma, # VPA�̌���
                    sim0=fres.HS, # �����\���̌���
                    future.function.name="future.vpa", # �����\���Ŏg���֐��̖��O�ifuture.vpa���g���ĉ������j
                    res1=HS.par0, # �Đ��Y�֌W�̃p�����[�^(sim0��fres.HS��^���Ă���ꍇ�A�����͎g�p����Ȃ�)
                    N=100, # ���ۂɌv�Z����ꍇ�́A���Ȃ��Ƃ��P���ȏ�̒l���g���ĉ�����
                    current.resid=5) # AR���胂�f�����g���ꍇ�A�����\���ɂ����ĉ��N���̎c�����l�����邩
# ���ʂ̗v��
MSY.HS2$summary
```

```
##                       Equiribrium with AR Fref/Fcurrent
## Bmsy                       127730  129244          0.52
## B_pgy_90%_L                 58417   59466          0.96
## B_limit (B_pgy_60%_L)       35362   35428          1.05
## B_ban (B_pgy_10%_L)          5582    5529          1.12
## Recent residual                NA      NA            NA
```

```r
## BH�����肷��ꍇ
MSY.BH2 <- est.MSY2(res.pma, # VPA�̌���
                    sim0=fres.BH, # �����\���̌���
                    future.function.name="future.vpa", # �����\���Ŏg���֐��̖��O�ifuture.vpa���g���ĉ������j
                    res1=BH.par0, # �Đ��Y�֌W�̃p�����[�^(sim0��fres.HS��^���Ă���ꍇ�A�����͎g�p����Ȃ�)
                    N=100, # ���ۂɌv�Z����ꍇ�́A���Ȃ��Ƃ��P���ȏ�̒l���g���ĉ�����
                    current.resid=5) # AR���胂�f�����g���ꍇ�A�����\���ɂ����ĉ��N���̎c�����l�����邩
```

- ���ʂ̕\�̐���

|     �Ǘ���l | ���t��Ԃɂ�����l| �ߔN�̎c�����l�����ĂT�N�ԏ����\�������Ƃ��̒l���Ǘ���l�Ƃ��Ďg��|Fcurrent�ɑ΂���搔| |
|:----------------------|:--------------------|:--------------------|:--------------------|:--------------------|
|                     | Equiribrium |with AR| Fref/Fcurrent|   |
|Bmsy                 |      128212|  128903  |        0.51| Bmsy=Btarget |
|Bpgy90%L           |      58664 |  58753  |        0.96| PGY90%��target�͈̔͂̉����Ƃ��čl������H|
|Blimit(Bpgy60%L) |      35326  | 35435    |      1.05| PGY60%��Blimit�̌��̈�� | 
|Bban(Bpgy10%L)   |       5558  |  5210    |      1.13| PGY10%��Bban�̌��ƂȂ�|
|Recent residual       |         NA   |   NA      |      NA| �ߔN�̎c�����l������ꍇ�A�ߔN�̎c���̒l������ |

Bmsy�̍s��Fref/Fcurrent�����s��F�����F�̍팸���ɂȂ�܂��i�iFref/Fcurrent-1)�~100�������]���[�̗v��\�́u�����F�l����̑������v�ɑ������܂��j�B���̒l�ɂ���Ƀ��iBtarget������m�����T�O������Blimit������m�����X�O���ȏ�ɂȂ�悤�ɒ�������W���j��(B-Bban)/(Blim-Bban)���悶��F�����Ƃ�ABC���Z�肳��܂�

## 8. HCR�̌v�Z��ABC�̎Z�o
### beta�̌v�Z
- HCR�ɂ��������calc.beta�֐�����v�Z���܂��B
- �����Ƃ���Btarget, Blimit, Bban��^����K�v������܂�


```r
beta <- calc.beta(res=MSY.HS2,# MSY�̌v�Z����
                  prob.beta=c(0.5,0.9), # Btarget, Blimit�����p�[�Z���g�̊m���ŏ��邩
                  Btar=MSY.HS2$Btar, # Btarget�̒l
                  Blim=MSY.HS2$Blim, # Blimit�̒l
                  Bban=MSY.HS2$Bban, # Bban�̒l
                  Fmsy=MSY.HS2$Fmsy) # Fmsy�̒l
```

```
## beta= 0.99
```

```r
beta[[1]]$beta # ���t��Ԃɂ�����SSB�̕��z�����K���z����O���(���ϒl�ƒ����l������邽��)�قǁE���U���傫���قǁiBlimit��90%�̊m���ŏ�������������Ă��邽�߁jbeta�̒l�͏������Ȃ�
```

```
## [1] 0.9928483
```

### HCR�����Ƃɏ����\����ABC�v�Z
- ���肳�ꂽHCR�̂��Ƃŏ����\���v�Z�������Ȃ��܂�
- �����]���ŏI�N�{�Q�N�ڂ́u���ρv���l�ʂ�ABC�Ƃ��܂�
- ���̂Ƃ��ɗp����e�������ʂ͎����]���ŏI�N�{�Q�N�ڂ̐e�������ʂł�


```r
input.abc <- fres.HS$input # �����\���̈����͈ȑO�̂��̂��g��
input.abc$multi <- MSY.HS2$Fmsy # ��{�Ƃ���F��Fmsy
input.abc$N <- 100 # ���ۂɌv�Z����Ƃ���10000�ȏ���g���Ă�������
input.abc$HCR <- list(Blim=MSY.HS2$Blim, Bban=MSY.HS2$Bban,beta=beta[[1]]$beta) # HCR�̃p�����[�^���w�肷��
input.abc$nyear <- 20
input.abc$is.plot <- TRUE
fres.abc1 <- do.call(future.vpa,input.abc)
```

![plot of chunk abc](figure/abc-1.png)

```r
hist(fres.abc1$ABC) # ABC�̕��z
ABC <- mean(fres.abc1$ABC) # ���ϒl��ABC�Ƃ���

## SSB�̏����\������
par(mfrow=c(1,1))
```

![plot of chunk abc](figure/abc-2.png)

```r
plot.future(fres.abc1,what=c(FALSE,TRUE,FALSE),is.legend=TRUE,lwd=2,
            col="darkblue",N=5,label=rep(NA,3))
draw.refline(MSY.HS2$summary,horiz=TRUE,lwd=1,scale=1)
```

![plot of chunk abc](figure/abc-3.png)

```r
## ���l�ʂ̏����\������
par(mfrow=c(1,1))
plot.future(fres.abc1,what=c(FALSE,FALSE,TRUE),is.legend=TRUE,lwd=2,
            col="darkblue",N=5,label=rep(NA,3))
points(rownames(fres.abc1$vssb)[2],ABC,pch=20,col=2,cex=3)
text(as.numeric(rownames(fres.abc1$vssb)[2])+1,ABC,"ABC",col=2)
```

![plot of chunk abc](figure/abc-4.png)

```r
## ���ۂɁA�ǂ��F�������\���Ŏg���Ă��邩
boxplot(t(fres.abc1$faa[1,,]/fres.abc1$faa[1,1,]),ylab="multiplier to current F")
```

![plot of chunk abc](figure/abc-5.png)


```r
# �ǂ��HCR�Ȃ̂������Ă݂�
ssb.abc <- mean(fres.abc1$vssb[2,]) # ABC�v�Z�N��ssb���Ƃ�
plot.HCR(alpha=beta[[1]]$beta,bban=MSY.HS2$Bban,blimit=MSY.HS2$Blim,btarget=MSY.HS2$Btar,lwd=2,
         xlim=c(0,MSY.HS2$Btar*2),ssb.cur=ssb.abc,Fmsy=MSY.HS2$Fmsy,yscale=0.7,scale=1000)
```

![plot of chunk HCR](figure/HCR-1.png)


```r
plot(apply(fres.abc1$vssb>MSY.HS2$summary[1,1],1,mean)*100,type="b",ylab="Probability",ylim=c(0,100))
points(apply(fres.abc1$vssb>MSY.HS2$summary[1,2],1,mean)*100,pch=2,type="b")
points(apply(fres.abc1$vssb>MSY.HS2$summary[3,1],1,mean)*100,pch=1,col=2,type="b")
points(apply(fres.abc1$vssb>MSY.HS2$summary[3,2],1,mean)*100,pch=2,col=2,type="b")
abline(h=c(50,90),col=c(1,2))
legend("bottomright",col=c(1,1,2,2),title="Probs",pch=c(1,2,1,2),legend=c(">Btarget_Eq",">Btarget_AR",">Blimit_Eq",">Blimit_AR"))
```

![plot of chunk probability](figure/probability-1.png)

## 9. ABC�v�Z�܂ł̂܂Ƃ߁i���Ԃ��Ȃ��l�͂�������X�^�[�g�j

MSY�Ǘ���l���v�Z�͈ȉ��̎菇�ł����Ȃ��܂��D

1. �f�[�^�̓ǂݍ���

```r
# �֐��̓ǂݍ��� ��  warning�܂��́u�x���v���o�邩������܂��񂪁C���̌㓮���Ă���Ζ�肠��܂���
source("../program/rvpa1.9.2.r")
source("../program/future2.1.r")

# �f�[�^�̓ǂݍ���
caa <- read.csv("caa_pma.csv",row.names=1)
waa <- read.csv("waa_pma.csv",row.names=1)
maa <- read.csv("maa_pma.csv",row.names=1)
dat <- data.handler(caa=caa, waa=waa, maa=maa, M=0.5)
names(dat)
```
2. VPA�̎��{(vpa)�@�� res.pma(VPA�̌���)�𓾂�
   - current F�Ƃ��Ăǂ̂悤�Ȓl���g�����A�����Őݒ肵�Ă����ifc.year�I�v�V�����ŁA���N���牽�N��F�𕽋ς��邩�w��)

```r
# VPA�ɂ�鎑���ʐ���
res.pma <- vpa(dat,fc.year=2009:2011,rec=585,rec.year=2011,tf.year = 2008:2010,
               term.F="max",stat.tf="mean",Pope=TRUE,tune=FALSE,p.init=1.0)
```
3. �Đ��Y�֌W�p�����[�^�̂��Ă͂� (fit.SR)�@��  HS.par0 (HS�ɂ��Ă͂߂��Ƃ��̃p�����[�^���茋��)�𓾂�
   - �c���̎��ȑ��ւ�����E�Ȃ������߂�B����ꍇ��AR=1�Ƃ����Ƃ��̌��ʂ�p���܂��B

```r
# VPA���ʂ��g���čĐ��Y�f�[�^�����
SRdata <- get.SRdata(res.pma)
head(SRdata)
```

```r
HS.par0 <- fit.SR(SRdata,SR="HS",method="L2",AR=0,hessian=FALSE)
HS.par1 <- fit.SR(SRdata,SR="HS",method="L2",AR=1,hessian=FALSE)
BH.par0 <- fit.SR(SRdata,SR="BH",method="L2",AR=0,hessian=FALSE)
BH.par1 <- fit.SR(SRdata,SR="BH",method="L2",AR=1,hessian=FALSE)
RI.par0 <- fit.SR(SRdata,SR="RI",method="L2",AR=0,hessian=FALSE)
RI.par1 <- fit.SR(SRdata,SR="RI",method="L2",AR=1,hessian=FALSE)
c(HS.par0$AICc,HS.par1$AICc,BH.par0$AICc,BH.par1$AICc,RI.par0$AICc,RI.par1$AICc)
```
4. HS.par0�����Ƃɏ����\�������{����(future.vpa) �� fres.HS (HS�����肵���Ƃ��̏����\������)�𓾂�
   - �����p�����[�^�𕽋ς���N,ABC�v�Z�N�Ȃǂ̃I�v�V������ݒ�
   - �����ʂƔN��ʂ̑̏d�ɑ��ւ�����ꍇ�͂���������\���̐ݒ�Ɏ�荞��(waa.fun=TRUE)

```r
fres.HS <- future.vpa(res.pma,
                      multi=1,
                      nyear=50, # �����\���̔N��
                      start.year=2012, # �����\���̊J�n�N
                      N=100, # �m���I�v�Z�̌J��Ԃ���
                      ABC.year=2013, # ABC���v�Z����N
                      waa.year=2009:2011, # �����p�����[�^�̎Q�ƔN
                      maa.year=2009:2011,
                      M.year=2009:2011,
                      is.plot=TRUE, # ���ʂ��v���b�g���邩�ǂ���
                      seed=1,
                      silent=TRUE,
                      recfunc=HS.recAR, # �Đ��Y�֌W�̊֐�
                      # recfunc�ɑ΂������
                      rec.arg=list(a=HS.par0$pars$a,b=HS.par0$pars$b,
                                   sd=HS.par0$pars$sd,resid=HS.par0$resid))
```
5. res.pma��fres.HS���g����MSY�Ǘ���l���v�Z���� (est.MSY2) �� MSY.HS2 (�Ǘ���l�̐��茋��)�𓾂�
   - �ŐV��est.MSY2�֐����g���ĉ�����

```r
par(mfrow=c(1,1))
# est.MSY2�ł͐��㎞�Ԃ̌v�Z�~�Q�O�N���������I�ɏ����\������悤�ɂȂ��Ă��܂�
MSY.HS2 <- est.MSY2(res.pma, # VPA�̌���
                    sim0=fres.HS, # �����\���̌���
                    future.function.name="future.vpa", # �����\���Ŏg���֐��̖��O�ifuture.vpa���g���ĉ������j
                    res1=HS.par0, # �Đ��Y�֌W�̃p�����[�^(sim0��fres.HS��^���Ă���ꍇ�A�����͎g�p����Ȃ�)
                    N=100, # ���ۂɌv�Z����ꍇ�́A���Ȃ��Ƃ��P���ȏ�̒l���g���ĉ�����
                    current.resid=5) # AR���胂�f�����g���ꍇ�A�����\���ɂ����ĉ��N���̎c�����l�����邩
# ���ʂ̗v��
MSY.HS2$summary

## BH�����肷��ꍇ
MSY.BH2 <- est.MSY2(res.pma, # VPA�̌���
                    sim0=fres.BH, # �����\���̌���
                    future.function.name="future.vpa", # �����\���Ŏg���֐��̖��O�ifuture.vpa���g���ĉ������j
                    res1=BH.par0, # �Đ��Y�֌W�̃p�����[�^(sim0��fres.HS��^���Ă���ꍇ�A�����͎g�p����Ȃ�)
                    N=100, # ���ۂɌv�Z����ꍇ�́A���Ȃ��Ƃ��P���ȏ�̒l���g���ĉ�����
                    current.resid=5) # AR���胂�f�����g���ꍇ�A�����\���ɂ����ĉ��N���̎c�����l�����邩
```

6. �Ǘ���l��������v�Z����icalc.beta�j

```r
beta <- calc.beta(res=MSY.HS2,# MSY�̌v�Z����
                  prob.beta=c(0.5,0.9), # Btarget, Blimit�����p�[�Z���g�̊m���ŏ��邩
                  Btar=MSY.HS2$Btar, # Btarget�̒l
                  Blim=MSY.HS2$Blim, # Blimit�̒l
                  Bban=MSY.HS2$Bban, # Bban�̒l
                  Fmsy=MSY.HS2$Fmsy) # Fmsy�̒l
beta[[1]]$beta # ���t��Ԃɂ�����SSB�̕��z�����K���z����O���(���ϒl�ƒ����l������邽��)�قǁE���U���傫���قǁiBlimit��90%�̊m���ŏ�������������Ă��邽�߁jbeta�̒l�͏������Ȃ�
```

7. ���肳�ꂽHCR��p����20�N���x�̏����\�������{���AABC�Z�o�N��ABC���v�Z����
   - 10�N���Btarget������m���Ȃǂ��v�Z

```r
input.abc <- fres.HS$input # �����\���̈����͈ȑO�̂��̂��g��
input.abc$multi <- MSY.HS2$Fmsy # ��{�Ƃ���F��Fmsy
input.abc$N <- 100 # ���ۂɌv�Z����Ƃ���10000�ȏ���g���Ă�������
input.abc$HCR <- list(Blim=MSY.HS2$Blim, Bban=MSY.HS2$Bban,beta=beta[[1]]$beta) # HCR�̃p�����[�^���w�肷��
input.abc$nyear <- 20
input.abc$is.plot <- TRUE
fres.abc1 <- do.call(future.vpa,input.abc)
hist(fres.abc1$ABC) # ABC�̕��z
ABC <- mean(fres.abc1$ABC) # ���ϒl��ABC�Ƃ���

## SSB�̏����\������
par(mfrow=c(1,1))
plot.future(fres.abc1,what=c(FALSE,TRUE,FALSE),is.legend=TRUE,lwd=2,
            col="darkblue",N=5,label=rep(NA,3))
draw.refline(MSY.HS2$summary,horiz=TRUE,lwd=1,scale=1)

## ���l�ʂ̏����\������
par(mfrow=c(1,1))
plot.future(fres.abc1,what=c(FALSE,FALSE,TRUE),is.legend=TRUE,lwd=2,
            col="darkblue",N=5,label=rep(NA,3))
points(rownames(fres.abc1$vssb)[2],ABC,pch=20,col=2,cex=3)
text(as.numeric(rownames(fres.abc1$vssb)[2])+1,ABC,"ABC",col=2)

## ���ۂɁA�ǂ��F�������\���Ŏg���Ă��邩
boxplot(t(fres.abc1$faa[1,,]/fres.abc1$faa[1,1,]),ylab="multiplier to current F")
```

```r
# �ǂ��HCR�Ȃ̂������Ă݂�
ssb.abc <- mean(fres.abc1$vssb[2,]) # ABC�v�Z�N��ssb���Ƃ�
plot.HCR(alpha=beta[[1]]$beta,bban=MSY.HS2$Bban,blimit=MSY.HS2$Blim,btarget=MSY.HS2$Btar,lwd=2,
         xlim=c(0,MSY.HS2$Btar*2),ssb.cur=ssb.abc,Fmsy=MSY.HS2$Fmsy,yscale=0.7,scale=1000)
```

```r
plot(apply(fres.abc1$vssb>MSY.HS2$summary[1,1],1,mean)*100,type="b",ylab="Probability",ylim=c(0,100))
points(apply(fres.abc1$vssb>MSY.HS2$summary[1,2],1,mean)*100,pch=2,type="b")
points(apply(fres.abc1$vssb>MSY.HS2$summary[3,1],1,mean)*100,pch=1,col=2,type="b")
points(apply(fres.abc1$vssb>MSY.HS2$summary[3,2],1,mean)*100,pch=2,col=2,type="b")
abline(h=c(50,90),col=c(1,2))
legend("bottomright",col=c(1,1,2,2),title="Probs",pch=c(1,2,1,2),legend=c(">Btarget_Eq",">Btarget_AR",">Blimit_Eq",">Blimit_AR"))
```


  
