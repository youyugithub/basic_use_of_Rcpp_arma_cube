# Basic use of Rcpp arma cube
basic examples on how to use cube in Rcpp armadillo

### Some helpful armadillo documents
<http://dirk.eddelbuettel.com/papers/RcppArmadillo-intro.pdf>   
<https://github.com/petewerner/misc/wiki/RcppArmadillo-cheatsheet>    
<http://arma.sourceforge.net/docs.html>

## get three dimensions
Source:
```
#include <RcppArmadillo.h>
using namespace Rcpp;
//[[Rcpp::depends(RcppArmadillo)]]
//[[Rcpp::export]]
IntegerVector test1(arma::cube AA){
  IntegerVector out(3);
  out(0)=AA.n_rows;
  out(1)=AA.n_cols;
  out(2)=AA.n_slices;
  return(out);
}
```
In R:
```
> A<-array(1:24,dim=c(2,3,4))
> test1(A)
[1] 2 3 4
```
## algebra
Source:
```
arma::cube test2(arma::cube AA){
  return(AA+AA);
}
```
In R:
```
> A<-array(1:24,dim=c(2,3,4))
> test2(A)
, , 1

     [,1] [,2] [,3]
[1,]    2    6   10
[2,]    4    8   12

, , 2

     [,1] [,2] [,3]
[1,]   14   18   22
[2,]   16   20   24

, , 3

     [,1] [,2] [,3]
[1,]   26   30   34
[2,]   28   32   36

, , 4

     [,1] [,2] [,3]
[1,]   38   42   46
[2,]   40   44   48
```
## subsetting cubes
Source:
```
#include <RcppArmadillo.h>
using namespace Rcpp;
//[[Rcpp::depends(RcppArmadillo)]]
//[[Rcpp::export]]
arma::cube test2(arma::cube AA){
  AA=AA.subcube(0,0,0,1,1,1);
  return(AA);
}
```
In R:
```
> AA<-array(1:27,dim=c(3,3,3))
> test2(AA)
, , 1

     [,1] [,2]
[1,]    1    4
[2,]    2    5

, , 2

     [,1] [,2]
[1,]   10   13
[2,]   11   14
```

## Extract vector from a cube
Source:
```
#include <RcppArmadillo.h>
using namespace Rcpp;
//[[Rcpp::depends(RcppArmadillo)]]
//[[Rcpp::export]]
arma::mat testslicing1(arma::cube AA){
  int numcol=AA.n_cols;
  int numrow=AA.n_rows;
  int numslice=AA.n_slices;
  arma::mat BB(numcol,numrow);
  arma::vec vv(numslice);
  int ii,jj;
  for(ii=0;ii<numrow;ii++){
    for(jj=0;jj<numcol;jj++){
      vv=AA.tube(ii,jj);
      BB(ii,jj)=dot(vv,vv);
    }
  }
  return(BB);
}
```
In R:
```
> AA<-array(1:5^3,dim=c(5,5,5))
> testslicing1(AA)
      [,1]  [,2]  [,3]  [,4]  [,5]
[1,] 19255 21930 24855 28030 31455
[2,] 19770 22495 25470 28695 32170
[3,] 20295 23070 26095 29370 32895
[4,] 20830 23655 26730 30055 33630
[5,] 21375 24250 27375 30750 34375
```
Note: use function "tube".
## Extract vector from a cube
To extract 1st/2nd dimension, try:    
```
#include <RcppArmadillo.h>
using namespace Rcpp;
//[[Rcpp::depends(RcppArmadillo)]]
//[[Rcpp::export]]
arma::mat testslicing2(arma::cube AA){
  return(AA.slice(1).row(1));
}
```
In R:
```
> AA<-array(1:5^3,dim=c(5,5,5))
> testslicing2(AA)
     [,1] [,2] [,3] [,4] [,5]
[1,]   27   32   37   42   47
```
