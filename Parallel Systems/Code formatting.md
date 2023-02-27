	### Initialize to null
``` c
int a = NULL;
char *a = NULL;
```

### Function overheads

``` java
static inline
// moves the code for the function into the line
// has to be in the same file, or the header
// like a cut and paste

//also like
#define FUNC(a,s,r,c) a[ r * s + 2 ] // will not type check

static inline double func(a, r, s){ // will typecheck
	return a[r * s] + 2;
}


//don't use functions, there's overhead if it's buried in nested for loops. 

```