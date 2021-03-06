CC = icc
CFLAGS = -g -O0
MPICC = mpicc
FORT = ifort
OBJS=$(NONMPIOBJS) $(MPIOBJS) $(MPIOMPOBJS)
NONMPIOBJS = serialpi omppi
MPIOBJS = mpiSRpi mpireducepi
MPIOMPOBJS = mpiomp

all:	$(OBJS)

run:
	@for f in $(NONMPIOBJS); do echo + $$f; time ./$$f; done
	@for f in $(MPIOBJS); do echo + $$f; time mpirun -np 4 $$f; done
	@for f in $(MPIOMPOBJS); do echo + $$f; time mpirun -np 2 $$f; done

serialpi: serialpi.c

mpiSRpi: mpiSRpi.c
	$(MPICC) mpiSRpi.c -o mpiSRpi

mpireducepi: mpireducepi.c
	$(MPICC) mpireducepi.c -o mpireducepi

omppi: omppi.c
	$(CC) -fopenmp omppi.c -o omppi

mpiomp: mpiomp.c
	$(MPICC) -fopenmp mpiomp.c -o mpiomp

mpi: mpisubmit.pbs
	cc mpireducepi.c -o $(MEMBERWORK)/<PROJID>/mpi.out
	cd $(MEMBERWORK)/<PROJID>
	qsub mpisubmit.pbs

mpisr: mpisubmit.pbs
	cc mpiSRpi.c -o $(MEMBERWORK)/<PROJID>/mpi.out
	cd $(MEMBERWORK)/<PROJID>
	qsub mpisubmit.pbs

omp: submit.pbs
	cc -mp omppi.c -o $(MEMBERWORK)/<PROJID>/pi.out
	cd $(MEMBERWORK)/<PROJID>
	qsub submit.pbs

xmpiomp: mpisubmit.pbs
	cc -mp mpiomp.c -o $(MEMBERWORK)/<PROJID>)/mpi.out
	cd $(MEMBERWORK)/<PROJID>
	qsub mpisubmit.pbs

clean:
	/bin/rm -f $(OBJS)
