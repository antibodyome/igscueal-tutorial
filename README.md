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

On OSX, openmpi can be installed using Homebrew. You'll need either XCode or gcc (via Homebrew) to install.

```bash
brew install openmpi
```

You may have to specify the compiler, which requires a couple of extra flags.

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
../test/test.fas
```

You'll then get something like the following.

```
### Loaded 100 sequences from ...
```

Next, you'll get two prompts for output files.

```
Save main screening results to (`igscueal/v2/IgSCUEAL/scripts/`) ../results/test_igscueal.tsv
```

```
Save alternative rearrangements to: (`igscueal/v2/IgSCUEAL/scripts/`) ../results/test_igscueal.alt.tsv
```

The progress will be spooled to the terminal, and should look something like this:

```
Screened 100/100 sequences. Elapsed time : 00:00:43. ETA : 00:00:00.  2.27 sequences/ second
```

## Accuracy on simulated data

The script `TabulateSimulations.py` assesses the accuracy of IgSCUEAL using simulated data, which are available in the `test` subdirectory. These were generated using [igh-simulation](https://github.com/antibodyome/igh-simulation).

```
usage: TabulateSimulations.py [-h] -i IGSCUEAL -r REARRANGEMENT [-w] [-e] [-l]

Parse IgSCUEAL

optional arguments:
  -h, --help            show this help message and exit
  -i IGSCUEAL, --igscueal IGSCUEAL
                        IgSCUEAL main TSV file
  -r REARRANGEMENT, --rearrangement REARRANGEMENT
                        IgSCUEAL rearrangement TSV file
  -w, --weak            Report weak matches
  -e, --errors          Report mis-matches
  -l, --latex           Output results in a LaTeX friendly format
```

e.g.

```
python3 python/TabulateSimulations.py -i results/simple_rearrangements.tsv -r results/simple_rearrangements.alt.tsv
```

### Simple rearrangements

IgSCUEAL2:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_rearrangements_igscueal.tsv -r results/simple_rearrangements_igscueal.alt.tsv
Results for V
	Correct             12060 (100 %)
	Alternative         0 (0 %)
	Mismatch (allele)   0 (0 %)
	Mismatch (gene)     0 (0 %)
	No result           0 (0 %)
Results for D
	Correct             11859 (98.3 %)
	Alternative         0 (0 %)
	Mismatch (allele)   0 (0 %)
	Mismatch (gene)     201 (1.67 %)
	No result           0 (0 %)
Results for J
	Correct             12060 (100 %)
	Alternative         0 (0 %)
	Mismatch (allele)   0 (0 %)
	Mismatch (gene)     0 (0 %)
	No result           0 (0 %)
```

IgSCUEAL1:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_rearrangements_igscueal1.tsv -r results/simple_rearrangements_igscueal1.alt.tsv
Results for V
	Correct             11135 (92.3 %)
	Alternative         916 (7.6 %)
	Mismatch (allele)   0 (0 %)
	Mismatch (gene)     9 (0.0746 %)
	No result           0 (0 %)
Results for D
	Correct             11713 (97.1 %)
	Alternative         0 (0 %)
	Mismatch (allele)   12 (0.0995 %)
	Mismatch (gene)     335 (2.78 %)
	No result           0 (0 %)
Results for J
	Correct             12020 (99.7 %)
	Alternative         39 (0.323 %)
	Mismatch (allele)   1 (0.00829 %)
	Mismatch (gene)     0 (0 %)
	No result           0 (0 %)

```

### Simulations with insertions/deletions

IgSCUEAL2:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_indels_10000_igscueal.tsv -r results/simple_indels_10000_igscueal.alt.tsv
Results for V
	Correct             9905 (99 %)
	Alternative         90 (0.9 %)
	Mismatch (allele)   5 (0.05 %)
	Mismatch (gene)     0 (0 %)
	No result           0 (0 %)
Results for D
	Correct             6597 (66 %)
	Alternative         398 (3.98 %)
	Mismatch (allele)   150 (1.5 %)
	Mismatch (gene)     2855 (28.6 %)
	No result           0 (0 %)
Results for J
	Correct             9385 (93.8 %)
	Alternative         569 (5.69 %)
	Mismatch (allele)   5 (0.05 %)
	Mismatch (gene)     41 (0.41 %)
	No result           0 (0 %)
```

IgSCUEAL1:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_indels_10000_igscueal1.tsv -r results/simple_indels_10000_igscueal1.alt.tsv
Results for V
	Correct             9499 (95 %)
	Alternative         495 (4.95 %)
	Mismatch (allele)   5 (0.05 %)
	Mismatch (gene)     1 (0.01 %)
	No result           0 (0 %)
Results for D
	Correct             6605 (66 %)
	Alternative         396 (3.96 %)
	Mismatch (allele)   148 (1.48 %)
	Mismatch (gene)     2851 (28.5 %)
	No result           0 (0 %)
Results for J
	Correct             9628 (96.3 %)
	Alternative         341 (3.41 %)
	Mismatch (allele)   3 (0.03 %)
	Mismatch (gene)     28 (0.28 %)
	No result           0 (0 %)
```

### Simulations with 40 mutations/sequence

IgSCUEAL2:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_indels_40_10000_igscueal.tsv -r results/simple_indels_40_10000_igscueal.alt.tsv
Results for V
	Correct             9048 (90.5 %)
	Alternative         942 (9.42 %)
	Mismatch (allele)   9 (0.09 %)
	Mismatch (gene)     1 (0.01 %)
	No result           0 (0 %)
Results for D
	Correct             4848 (48.5 %)
	Alternative         634 (6.34 %)
	Mismatch (allele)   168 (1.68 %)
	Mismatch (gene)     4350 (43.5 %)
	No result           0 (0 %)
Results for J
	Correct             8307 (83.1 %)
	Alternative         1628 (16.3 %)
	Mismatch (allele)   17 (0.17 %)
	Mismatch (gene)     48 (0.48 %)
	No result           0 (0 %)
```

IgSCUEAL1:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_indels_40_10000_igscueal1.tsv -r results/simple_indels_40_10000_igscueal1.alt.tsv
Results for V
	Correct             8709 (87.1 %)
	Alternative         1248 (12.5 %)
	Mismatch (allele)   25 (0.25 %)
	Mismatch (gene)     18 (0.18 %)
	No result           0 (0 %)
Results for D
	Correct             4012 (40.1 %)
	Alternative         683 (6.83 %)
	Mismatch (allele)   233 (2.33 %)
	Mismatch (gene)     5072 (50.7 %)
	No result           0 (0 %)
Results for J
	Correct             8323 (83.2 %)
	Alternative         1550 (15.5 %)
	Mismatch (allele)   50 (0.5 %)
	Mismatch (gene)     77 (0.77 %)
	No result           0 (0 %)

```

### Simulations with 80 mutations/sequence

IgSCUEAL2:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_indels_80_10000_igscueal.tsv -r results/simple_indels_80_10000_igscueal.alt.tsv
Results for V
	Correct             8060 (80.6 %)
	Alternative         1923 (19.2 %)
	Mismatch (allele)   13 (0.13 %)
	Mismatch (gene)     4 (0.04 %)
	No result           0 (0 %)
Results for D
	Correct             2800 (28 %)
	Alternative         644 (6.44 %)
	Mismatch (allele)   173 (1.73 %)
	Mismatch (gene)     6379 (63.8 %)
	No result           0 (0 %)
Results for J
	Correct             7415 (74.2 %)
	Alternative         2460 (24.6 %)
	Mismatch (allele)   10 (0.1 %)
	Mismatch (gene)     111 (1.11 %)
	No result           0 (0 %)
```

IgSCUEAL1:

```bash
$ python3 python/TabulateSimulations.py -i results/simple_indels_80_10000_igscueal1.tsv -r results/simple_indels_80_10000_igscueal1.alt.tsv 
Results for V
	Correct             7811 (78.1 %)
	Alternative         2080 (20.8 %)
	Mismatch (allele)   48 (0.48 %)
	Mismatch (gene)     61 (0.61 %)
	No result           0 (0 %)
Results for D
	Correct             2095 (20.9 %)
	Alternative         568 (5.68 %)
	Mismatch (allele)   233 (2.33 %)
	Mismatch (gene)     7084 (70.8 %)
	No result           0 (0 %)
Results for J
	Correct             7147 (71.5 %)
	Alternative         2515 (25.1 %)
	Mismatch (allele)   83 (0.83 %)
	Mismatch (gene)     235 (2.35 %)
	No result           0 (0 %)

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

Example 1:

```bash
python3 python/IgPostProcessor.py -i results/simple_indels_80_10000_igscueal.tsv -s 0.01 -p CDR3_AA '^([^X\?]+|[^X\?]*\??[^X\?]*)$' -c JUNCTION_AA g 7 -r --coverage FW1,CDR1,FW2,CDR2,FW3,CDR3,J,CH > results/simple_indels_80_10000_igscueal.json
```

This will generate a JSON file for `results/simple_indels_80_10000_igscueal.tsv`, with a minimum support of 0.01 (`-s 0.01`), with a CDR3 region that matches the regular expression `^([^X\?]+|[^X\?]*\??[^X\?]*)$`, with a junction of greater than 8 amino acids (`-c JUNCTION_AA g 7`), summarising all reads by V/D/J usage (`-r`), counting how many reads map to a region.

Example 2:

```bash
python3 python/IgPostProcessor.py -i results/simple_indels_80_10000_igscueal.tsv -s 0.01 -p CDR3_AA '^([^X\?]+|[^X\?]*\??[^X\?]*)$' -c JUNCTION_AA g 7 -o json 'Mapped Read' 'Best Rearrangement' JUNCTION > results/simple_indels_80_10000_igscueal.clusters.json
```

As above, but now output is a JSON file comprising of the mapped read, the best rearrangement, and the junction for each read (`-o json 'Mapped Read' 'Best Rearrangement' JUNCTION`).

### ClonotypeProcessor.py

```bash
usage: ClonotypeProcessor.py [-h] -i [INPUT] -o [OUTPUT]

Process clonotypes from a binned JSON.

optional arguments:
  -h, --help            show this help message and exit
  -i [INPUT], --input [INPUT]
                        The input JSON file
  -o [OUTPUT], --output [OUTPUT]
                        The output JSON file
```

This takes the output from `IgPostProcessor` (see example 2 above) and generates clonotypes e.g.

```bash
python3 python/ClonotypeProcessor.py -i results/simple_indels_80_10000_igscueal.clusters.json -o results/simple_indels_80_10000_igscueal.reduced.json
```

This can be further processed using `IgPostProcessor`.

```bash
python3 python/IgPostProcessor.py -i results/simple_indels_80_10000_igscueal.tsv -s 0.01 -p CDR3_AA '^([^X\?]+|[^X\?]*\??[^X\?]*)$' -c JUNCTION_AA g 7 -x results/simple_indels_80_10000_igscueal.reduced.json -r --coverage FW1,CDR1,FW2,CDR2,FW3,CDR3,J,CH > results/simple_indels_80_10000_igscueal.clonotypes.json
```

Through specifying `-x results/simple_indels_80_10000_igscueal.reduced.json` only sequences that represent unique clonotypes are kept.

## Using your own references

Currently, v2 only contains references for human IGH sequences. Documentation on how to add your own data will be forthcoming.
