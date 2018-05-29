# basic_use_of_Rcpp_arma_cube
basic examples on how to use cube in Rcpp armadillo


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
