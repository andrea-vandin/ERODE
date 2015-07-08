# ERODE
Exact Reduction of ODE systems

_*ERODE*_ is a Java tool which interacts with the state-of-the-art Micrsoft's SMT solver [Z3](https://github.com/Z3Prover/z3) to perform automatic exact reductions of chemical reaction networks (CRN).

The tool exploits two novel reduction techniques, the Forward and Backward Differential Equivalences, defined for Intermediate Drift Oriented (IDOL) programs. IDOL essentially allows to express systems of nonlinear Ordinary Differential Equations (ODE). Hence, the reduction techniques can be applied to the well-known continuous-state semantics of CRNs based on the law of mass action, associating an ODE to each species of the CRN. 

A Forward Differential Equivalence (FDE) is an equivalence among ODE variables (i.e., species of a CRN) characterizing the notion of [ODE lumpability](http://epubs.siam.org/doi/abs/10.1137/S0036139995293294?journalCode=smjmap): a reduced ODE system can be given expressed in terms of an ODE variable per block of an FDE partition. 

A Backward Differential Equivalence (BDE) is similar, but it characterizes the notion of [Exact Fluid lumpability](http://link.springer.com/chapter/10.1007%2F978-3-642-32940-1_27): equivalent ODE variables have the same solution at all time points; in other words, a BDE relates species whose ODE solutions are equal whenever they start from identical initial conditions. 

The tool currently supports CRNs given in the .net format generated with the well-established tool [BioNetGen](http://www.bionetgen.org/index.php/Main_Page). This allowed us to validate our differential equivalences against a wide set of existing models in the literature.

A first prototype of our tool is available at [ERODE](https://copy.com/tmextXxmbJ32ZIGW). 

Currently, it has to be intended only as accompanying material for the paper presenting our differential equivalences submitted at [POPL 2016](http://conf.researchr.org/home/POPL-2016): [Satisfiability Modulo Differential Equivalence Relations](https://www.dropbox.com/s/005svfue6eozle8/z3-popl16.pdf?dl=0).

For the sake of reproducibility, ERODE comes with the models M1-M12 treated in the draft, stored in the folder "BNGNetworks".

Our differential equivalences can be applied to any .net model by just running the following command (on a Mac machine)

```
#!bash
         ./runOnMac.sh fileIn technique fileOut
```

where *fileIn* specifies the .net file containing the CRN to be reduced up to our differential equivalences, which is automatically converted in an IDOL model, while *technique* can be either BDE or FDE. In case the BDE technique is used, the ODE variables (i.e., the species of the input CRN) are additionally pre-partitioned in blocks of species with same initial conditions. Finally, the parameter *fileOut* specifies a text file where to store information about the computed partition.

For example, these are the commands to reduce the CRN M11 of the draft using BDE and FDE, respectively:

```
#!bash
         ./runOnMac.sh ./BNGNetworks/M7.net BDE BDEofM7.txt

         ./runOnMac.sh ./BNGNetworks/M7.net FDE FDEofM7.txt
```

It is worth to remark that M7 is a special case where the two computed partitions coincide.

----
###Example: reducing the model M7 of the draft up to Backward Diffrential Equivalence###


```
#!bash
./runOnMac.sh ./BNGNetworks/M7.net BDE BDEofM7.txt

Importing: ./BNGNetworks/M7.net
Read parameters: 9, read species: 18, read CRNreactions: 48, read reagents: 26, read products: 36. 

Prepartinioning with respect to the initial concentrations of the species... completed. From 18 species to 3 blocks. Time necessary: 0.004 (s)

BDE partitioning ./BNGNetworks/M7.net ... (Total SMT init time: 0.094, total SMT check time: 0.04400000000000001 (s) ) completed. From 18 species to 12 blocks. Time necessary: 0.203 (s).

Writing the partition in file ./BDEofM7.txt ... completed
```

Where the file BDEofM7.txt will contain the following partition of the species of M7:
```
#!bash
The partition has 12 blocks out of 18 species:
Block 1, Size: 1
2-S(p1~U,p2~U) 

Block 2, Size: 2
8-E(s!1).S(p1~U!1,p2~P) 
9-E(s!1).S(p1~P,p2~U!1) 

Block 3, Size: 1
0-E(s) 

Block 4, Size: 1
1-F(s) 

Block 5, Size: 2
3-E(s!1).S(p1~U!1,p2~U) 
4-E(s!1).S(p1~U,p2~U!1) 

Block 6, Size: 1
5-E(s!1).E(s!2).S(p1~U!1,p2~U!2) 

Block 7, Size: 2
6-S(p1~P,p2~U) 
7-S(p1~U,p2~P) 

Block 8, Size: 2
12-E(s!1).F(s!2).S(p1~U!1,p2~P!2) 
14-E(s!1).F(s!2).S(p1~P!2,p2~U!1) 

Block 9, Size: 1
13-S(p1~P,p2~P) 

Block 10, Size: 2
15-F(s!1).S(p1~P,p2~P!1) 
16-F(s!1).S(p1~P!1,p2~P) 

Block 11, Size: 1
17-F(s!1).F(s!2).S(p1~P!1,p2~P!2) 

Block 12, Size: 2
10-F(s!1).S(p1~P!1,p2~U) 
11-F(s!1).S(p1~U,p2~P!1) 
```
