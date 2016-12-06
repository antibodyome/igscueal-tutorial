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

Enter the path of the sequence alignment file. Test datasets are included in the repository, in the `test` subdirectory. Paths are relative to the script, so you can type:

```
../test/simple_indels_80_10000.fas
```

You'll then get something like the following.

```
### Loaded 10000 sequences from ...
```

Next, you'll get two prompts for output files.

```
Save main screening results to (`igscueal/v2/IgSCUEAL/scripts/`) ../test/simple_indels_80_10000_igscueal.txt
```

```
Save alternative rearrangements to: (`igscueal/v2/IgSCUEAL/scripts/`) ../test/simple_indels_80_10000_igscueal_rearrangements.txt
```

The progress will be spooled to the terminal, and should look something like this:

```
Screening read 9996/10000. Elapsed time : 00:25:10. ETA : 00:00:00.  6.62 sequences/ second.
```

## Postprocessing

### IgPostProcessor

```
usage: IgPostProcessor.py [-h] -i [INPUT] [-s SUPPORT] [-p PATTERN PATTERN]
                          [-c COLUMN COLUMN COLUMN] [-x CLUSTER]
                          [-o OUTPUT [OUTPUT ...]] [-l LENGTH] [-r]
                          [-g COVERAGE] [-t] [-a ALTERNATIVES] [-v]

Post-process IgSCUEAL output files.

optional arguments:
  -h, --help            show this help message and exit
  -i [INPUT], --input [INPUT]
                        The tab-separated file produced by IgSCUEAL. Must have
                        at least 10 columns
  -s SUPPORT, --support SUPPORT
                        Minimum assignment support required to process a read
                        (default is 0.9)
  -p PATTERN PATTERN, --pattern PATTERN PATTERN
                        A regular expression pattern for pulling out reads
                        that match a particular pattern in a particular
                        column, e.g. "Rearrangement V3-21,J3"
  -c COLUMN COLUMN COLUMN, --column COLUMN COLUMN COLUMN
                        A column [l,g,e] length argument to select lines based
                        on the length of the string in a given column, e.g.
                        JUNCTION_AA g 10 (all lines with JUNCTIONS_AA with
                        more than 10 chars)
  -x CLUSTER, --cluster CLUSTER
                        Only keep sequences which represent unique clonotypes
  -o OUTPUT [OUTPUT ...], --output OUTPUT [OUTPUT ...]
                        Output mode (default fasta): either summarize filtered
                        reads by column value (supply the column name), print
                        FASTA for the reads matching selection criteria
                        (fasta), or output JSON (json) for matching reads.
                        Finally specify "json output bin [bin2...]" to output
                        a json of "ID": "output" for each unique value of
                        "bin+bin2" (group by)
  -l LENGTH, --length LENGTH
                        Summarize the lengths of sequences in a given column
  -r, --rearrangement   Summarize all reads by [H (family, gene, allele) : D
                        (gene-allele) : J (gene, allele)]
  -g COVERAGE, --coverage COVERAGE
                        Count how many reads passing the filter contain a
                        mapped region; the columns to be counted are provided
                        as a comma separated list; works in conjunction with
                        --rearrgement
  -t, --light_chain     If set, assume that the reads come from a light Ig
                        chain (no d-region)
  -a ALTERNATIVES, --alternatives ALTERNATIVES
                        If set, read alternative rearrangements from this file
                        and use those for rearranegment binning
  -v, --inverse_d       If set (for heavy chains only), produce the D-region
                        assignment by also considering inverted regions
```


## Using your own references
