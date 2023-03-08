

```Setup
module list
module unload openmpi
module load ucx openshmem
make
```

```Run Command
srun -n 11 lab3 -n 11 -t 10
```


```remove
scancel id
```



Terms
- Super non linear performance ( impossible cuz parallel only adds overhead and communication)
- Eureka problems


