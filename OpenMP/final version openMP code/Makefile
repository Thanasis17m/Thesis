OMPFLAG = -fopenmp
#PROFLAG = -pg
CC = g++
#CC = /home/elias/gcc-bin/bin/g++-4.7.1
NMAX ?= 1002
DEFS = -DNMAX=$(NMAX) -D_PRECALC_
OPTFLAG = -O2 -fomit-frame-pointer -ftree-vectorize -ftree-vectorizer-verbose=1 -msse2 -msse -mfpmath=sse -funroll-loops -fassociative-math -fno-signed-zeros -fno-trapping-math -fno-signaling-nans --param vect-max-version-for-alias-checks=20
FLAGS_SEQ = ${OPTFLAG} ${PROFLAG} ${DEFS} -Wall -g -I../common
FLAGS = ${FLAGS_SEQ} ${OMPFLAG}
LFLAGS_SEQ = ${PROFLAG}
LFLAGS = ${PROFLAG} ${OMPFLAG}
LIBS = -lm -lrt
CLEAN_FILES = lmsor_rb_seq.o lmsor_rb_seq_sse2.o lmsor_rb_omp.o lmsor_rb_omp_sse2.o lmsor_seq lmsor_seq_sse2 lmsor_omp lmsor_omp_sse2

all: lmsor_seq lmsor_seq_sse2 lmsor_omp lmsor_omp_sse2

lmsor_seq: lmsor_rb_seq.o
	${CC} ${LFLAGS_SEQ} -o $@ lmsor_rb_seq.o ${LIBS}

lmsor_seq_sse2: lmsor_rb_seq_sse2.o
	${CC} ${LFLAGS_SEQ} -o $@ lmsor_rb_seq_sse2.o ${LIBS}

lmsor_omp: lmsor_rb_omp.o
	${CC} ${LFLAGS} -o $@ lmsor_rb_omp.o ${LIBS}

lmsor_omp_sse2: lmsor_rb_omp_sse2.o
	${CC} ${LFLAGS} -o $@ lmsor_rb_omp_sse2.o ${LIBS}

lmsor_rb_seq.o: parallel_for.cpp
	${CC} -c ${FLAGS_SEQ} -DMODIFIED_SOR parallel_for.cpp -o $@

lmsor_rb_seq_sse2.o: parallel_for.cpp
	${CC} -c ${FLAGS_SEQ} -DMODIFIED_SOR -D_INTRINSIC_SSE2_ parallel_for.cpp -o $@

lmsor_rb_omp.o: parallel_for.cpp
	${CC} -c ${FLAGS} -DMODIFIED_SOR -DBLOCK_PARTITIONING parallel_for.cpp -o $@

lmsor_rb_omp_sse2.o: parallel_for.cpp
	${CC} -c ${FLAGS} -DMODIFIED_SOR -DBLOCK_PARTITIONING -D_INTRINSIC_SSE2_ parallel_for.cpp -o $@

clean:
	\rm -f $(CLEAN_FILES)

rebuild: clean all
