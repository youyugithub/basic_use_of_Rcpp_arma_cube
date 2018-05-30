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
