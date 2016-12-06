# igscueal-tutorial: A Tutorial for IgSCUEAL v2

## Prerequisities

### HyPhy

You will need the v2.3-dev branch of HyPhy to run IgSCUEAL v2, which can be downloaded from GitHub. You will also need to have MPI libraries (e.g. openmpi installed).

```bash
git clone https://github.com/veg/hyphy
cd hyphy
git checkout v2.3-dev
cmake .
make MPI
sudo make install
```

On OSX, you may need to specify use of gcc rather than clang as the compiler. gcc5 and openmpi can be installed using Homebrew.

```bash
brew install gcc5
brew install openmpi
```

Specifying the compiler requires a couple of extra flags.

```bash
cmake -DCMAKE_C_COMPILER=gcc-5 -DCMAKE_CXX_COMPILER=g++-5
```

### IgSCUEAL

To use v2 of IgSCUEAL, you will need to download the repository and switch to the v2 branch.

```bash
git clone https://github.com/antibodyome/IgSCUEAL
cd IgSCUEAL
git checkout v2
```

## Running

To run e.g. using 12 processes:

```bash
mpirun -np 12 /usr/local/bin/HYPHYMPI scripts/screen_fasta.bf
```

You'll be greeted with a prompt for the filename of a FASTA file.

```markdown
Analysis Description
--------------------
IgSCUEAL (Immunoglobulin subtype classification using evolutionary
algorithms) uses a phylogenetic placement algorithm to assign a
rearrangement to an Ig* (human) read V, J, and CH1 are classified this
way; D region is assigned based on best nucleotide homology.

- __Requirements__: a FASTA file with (human) Ig reads

- __Citation__: Frost SDW, et al (2015) *Assigning and visualizing germline genes in
antibody repertoires* (Phil. Trans. R. Soc. B 2015 370)

- __Written by__: Sergei L Kosakovsky Pond

- __Contact Information__: spond@temple.edu

- __Analysis Version__: 2.00


Select a sequence alignment file (`igscueal/v2/IgSCUEAL/scripts/`)
```

Enter the path of the sequence alignment file. A simulated dataset is included in the repository. Paths are relative to the script, so you can type:

```
../simple_indels_80_10000.fas
```

You'll then get something like the following.

```
### Loaded 10000 sequences from ...
```

Next, you'll get two prompts for output files.

```
Save main screening results to (`igscueal/v2/IgSCUEAL/scripts/`) ../simple_indels_80_10000_igscueal.txt
```

```
Save alternative rearrangements to: (`igscueal/v2/IgSCUEAL/scripts/`) ../simple_indels_80_10000_igscueal_rearrangements.txt
```

## Using your own references
