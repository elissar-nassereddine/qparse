# *QPARSE* - 
QPARSE is a Python program (Python v. 2.7) that allows to search for putative patterns associated with the formation of four-stranded non-canonical DNA structure (PQS).
 
testo

## **Contacts**
Michele Berselli, <berselli.michele@gmail.com>  

Feel free to contact me for any inquiry, unexpected behaviour or support for your analyses. 

## **QPARSE.py**
QPARSE_x.x.py 
 
####Requirements
QPARSE requires Python (version 2.7) and the numpy library.

To install numpy under unix environment (linux, osx):  

	sudo pip install numpy

To get help with numpy installation check 
[numpy docs](https://docs.scipy.org/doc/ "numpy documentation")

####Running QPARSE

To run QPARSE under unix environment (linux, osx):

	# move to QPARSE folder
	cd PATH/TO/QPARSE/FOLDER/
	
	# make QPARSE executable
	sudo chmod +x QPARSE_x.x.py
	
	# run QPARSE
	./QPARSE_x.x.py -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE [OPTIONS]

	# !! if the above is not working run
	python QPARSE_x.x.py -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE [OPTIONS]


####Input
The program accepts in input both fasta and multi-fasta files.

####Parameters
The tool is very flexible and allows the user to select different parameters that can be defined to refine and customize the analysis. The parameters that can be used are:

***note**: defaults are shown in [], bool parameters do not require an argument. Parameters accepting numbers require integers.*

######Basic parameters:  - **-i**, **--inputsequence** PATH/TO/INPUTFILE: *Input file with sequence/s as fasta/multi-fasta.*  - **-o**, **--output** PATH/TO/OUTPUTFILE: *Output file to use for writing results.*  - **-b**, **--base** BASE [G]: *Letter that is used to detect the islands. G is used by default but other letters can be selected and used for the analysis instead (case-sensitive, use UPPERCASE).*
  - **-m**, **--minlen** MINLEN [2]: *This parameter defines the minimum length for the islands. It is also possible to select a range of lengths by adding the parameter* **–M**, **--maxlen** MAXLEN *to detect all the islands that are in the range of length* [m..M]*. While all the possible islands of the different lengths in the range are detected, only islands of the same length can be part of the same PQS.*
  - **-L**, **--maxloop** MAXLOOP [12]: *This parameter defines the maximum distance (loop distance) that is allowed between two consecutive islands within the same PQS.*
  - **-g**, **--gapnum** GAPNUM [0]: *This parameter defines the maximum number of gaps that can be opened per island. Alternatively, or in combination, the parameter* **–l**, **--gaplen** GAPLEN [0] *defines the maximum cumulative length of the gaps that is permitted per island. Finally, the parameter* **–nocore** (bool) *defines whether at least two consecutive bases are required to define an island. This Boolean parameter allows to detect also islands that do not have a “core” of at least two consecutive bases (e.g.G, GtGaG).*
  - **-n**, **--islandnum** ISLANDNUM [4]: *This parameter defines the number of islands that are required to be consecutive in the same PQS.*  - **-p**, **--perfect** (bool) [1]: *This parameter defines the minimum number of islands that are required to be “perfect” within each PQS. Since the tool allows to detect also islands that are degenerate, this parameter imposes a constraint for the minimum number of islands that cannot contain any degeneration within each pattern. If also PQS with no perfect islands are required use the parameter* **-noperfect**, **--noperfect** (bool)*.*
  -	**-fast**, **--fastermode** (bool): *This parameter triggers a faster search mode that can be applied for islandnum > 4. Only regions with at least* **--islandnum** *islands are evaluated to build the graph, however, the graph is navigated searching for patterns of four islands to reduce the combinatorial complexity and speed up the analysis.*
  
######Loop symmetry check:
  -	**-sM**, **--simmetrymirror** (bool): *Evaluate the symmetry of the long loops (>= 6) to improve the score, MIRROR symmetry is considered. Allows to detect longer loops with mirror properties that can form Hoogsteen-hairpins.*
  -	**-sP**, **--simmetrypalindrome** (bool): *Evaluate the symmetry of the long loops (>= 6) to improve the score, PALINDROMIC symmetry is considered. Allows to detect longer loops with palindromic properties that can form canonical-hairpins.*
  -	**-sX**, **--simmetrymixed** (bool): *Evaluate the symmetry of the long loops (>= 6) to improve the score, MIXED MIRROR-PALINDROMIC symmetry is considered. Allows to detect longer loops with mirror and palindromic properties that can form mixed-hairpins.*
  -	**-sC**, **--simmetrycustom** PATH/TO/CUSTOM_MATRIX: *Evaluate the symmetry of the long loops (>= 6) to improve the score, the custom substitution-matrix specified is used. Allows to detect longer loops with symmetrical properties that can form hairpins.*

######Parameters to be used with caution:
  -	**-all**, **--allresult** (bool): *This parameter allows to show all the possible patterns that are detected, also overlapping and suboptimal [caution!: the output can be huge].*
  -	**-normal**, **--normalmode** (bool): *Searching for more than 12 islands* **--fastermode** *is used by default, this parameter override* **--fastermode** *and the graph is navigated searching for patterns of* **--islandnum** *islands [caution!: the analysis can be computationally expensive].*


####Examples of command lines
  - Perfect PQS with 4 islands of length 3 bp (-m), maximum loop distance of 7 bp (-L). Default search for G, -b C can be added to search for C:  
  
		./QPARSE_x.x -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE -m 3 -L 7 [-b C]

  - Perfect PQS with 4 islands of length in range [3..4] bp (-m, -M), maximum loop distance of 7 bp (-L). Default search for G, -b C can be added to search for C:  
  
		./QPARSE_x.x -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE -m 3 -M 4 -L 7 [-b C]

  - Degenerate PQS with 4 islands of length in range [3..4] bp (-m, -M), maximum loop distance of 7 bp (-L). Islands can have a maximum of 1 gap (-g) and a maximum gap length of 2 bp (-l). Default search for G, -b C can be added to search for C:  
  
		./QPARSE_x.x -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE -m 3 -M 4 -L 7 -g 1 -l 2  [-b C]

  - Degenerate PQS with 4 islands of length in range [3..4] bp (-m, -M), maximum loop distance of 7 bp (-L). Islands can have a maximum of 2 gap (-g) and a maximum gap length of 3 bp (-l). Also islands that do not have a “core” of at least two consecutive bases (-nocore) are detected (e.g. GcGtG). Default search for G, -b C can be added to search for C:  
  
		./QPARSE_x.x -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE -m 3 -M 4 -L 7 -g 2 -l 3 -nocore [-b C]

  - Degenerate PQS with 8 islands (-n) of length in range [3..4] bp (-m, -M), maximum loop distance of 7 bp (-L). Islands can have a maximum of 1 gap (-g) and a maximum gap length of 2 bp (-l). At least 5 non-degenerate islands (-p) are required. Default search for G, -b C can be added to search for C:  
  
		./QPARSE_x.x -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE -m 3 -M 4 -L 7 -g 1 -l 2 -n 8 -p 5 [-b C]
	
  - Degenerate PQS with 4 islands of length in range [3..4] bp (-m, -M), maximum loop distance of 7 bp (-L). Islands can have a maximum of 1 gap (-g) and a maximum gap length of 2 bp (-l). Long loops (>= 6 bp) are analyzed for mixed mirror-palindromic symmetrical properties (-sX). Default search for G, -b C can be added to search for C:  
  
		./QPARSE_x.x -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE -m 3 -M 4 -L 7 -g 1 -l 2 -sX [-b C]
	
####Output
######Standard output

	#quadruplex     score   start   end     island_len
	>GeneID_1
	GGGG-tgt-GGGG-aca-GGGG-tgt-GGGG 40      3       27      4
	>GeneID_2
	GTGG--GGG--GGATG-taggt-GGG      16      3       22      3
	GTGG--GGG-ggat-GTAGG-t-GGG      16      3       22      3
	GTGG-g-GGG-gat-GTAGG-t-GGG      16      3       22      3
	GTGG-gg-GGG-at-GTAGG-t-GGG      16      3       22      3
	GGG--GGG-gat-GTAGG-t-GGG        19      5       22      3
	GGG-g-GGG-at-GTAGG-t-GGG        19      5       22      3
	GGG--GGG-at-GTAGG-t-GGG 19      6       22      3

The program returns a standard output that is structured in blocks. Each block corresponds to one of the sequences analyzed. Each block starts with a fasta-like header containing the information of the sequence as provided in the fasta input. The subsequent lines represent the detected PQS. Each line contains the sequence, the score, the start and the end indexes, and the length of the islands for the PQS. The values are separated by TAB. In the PQS sequence the islands are represented in CAPS LOCK and are connected to loops by '-': 'ISL-loop-ISL-loop-ISL-loop-...-ISL'. Null loops are represented as 'ISL--ISL'.

######Output with loop symmetry check

	#quadruplex     score   start   end     island_len      loops_symmetry
	>GeneID_1
	GGCG-tta-GGG-aa-GGG-cgtcgaaagca-GGG     19      2       30      3       NA;NA;HmWuWm
	GGCG-tta-GGG-aa-GGG-cgtcgaaagcagggt-GGG 20      2       34      3       NA;NA;HlHHWHWll
	GGCG-tta-GGG-aagggcgtcgaaagca-GGG-t-GGG 19      2       34      3       NA;WWmmHWuHu;NA
	GGCG-ttagggaa-GGG-cgtcgaaagca-GGG-t-GGG 22      2       34      3       HmWW;HmWuWm;NA
	GGG-aa-GGG-cgtcgaaagca-GGG-t-GGG        22      9       34      3       NA;HmWuWm;NA
	>GeneID_2
	GGG-ac-GGG-ggccggc-GGG-ccac-GGG 27      3       27      3       NA;WWWu;NA
	GGG-acg-GGG-gccggc-GGG-ccac-GGG 30      3       27      3       NA;WWW;NA
	GGG-acgg-GGG-ccggc-GGG-ccac-GGG 24      3       27      3       NA;NA;NA

When the symmetry of the loops is considered, for each PQS an additional field is reported in the output. This field contains the information of the self-alignment for each of the loops. The self-alignment encodings for each loop are separated by ';'. In the encodings, 'W' identifies a Watson-Crick pairing, 'H' a Hoogsteen pairing, 'l-u' a gap opening and 'm' a mismatch. If the loop is shorter than 6 bp and the alignment is not calculated, 'NA' is reported instead. This field is used to show the optimal alignment when using QPARSE_parser.py (see below).
  
## **QPARSE_parser.py**
Together with QPARSE, a python script is also provided that can be used to better organize the raw output.

####Command line

	./QPARSE_parser_x.x.py -i PATH/INPUT/FILE -o PATH/OUTPUT/FILE [-g] [-m] [-x] [-s] [-a] [-maxLoop MAXLOOP]

####Parameters
***note**: defaults are shown in [], bool parameters do not require an argument. Parameters accepting numbers require integers.*

######General parameters
  - **-i**, **--inputfile** PATH/TO/INPUTFILE: *Output file from QPARSE as input.* 
  - **-o**, **--outputfile** PATH/TO/OUTPUTFILE: *File to store the formatted output.*  
  - **-g**, **--gff** (bool): *By default the output of the parser is in tsv (TAB separated) format, this parameter converts the output to gff format.*  
  - **-m**, **--merge** (bool): *Using this parameter the overlapping PQS are merged to return the longest regions of consecutive islands whithin the loop distance.* 
  - **-x**, **--max** (bool): *Using this parameter only the maximum number of non overlapping PQS is returned.*
  - **-maxLoop**, **--maxLoop** MAXLOOP: *This parameter allows to define a maximum number of long loops (>= 6 bp) that is permitted in the results. The PQS with a higher number of long loops are filtered out.*

######Parameters to use when loops symmetries are considered
  - **-s**, **--score** (bool): *This parameter allows to order the PQS by score.*  
  - **-a**, **--alignment** (bool): *This parameter allows to order the PQS by score, and shows the optimal alignment calculated for the linking loops.*

####Output

######Standard tsv (TAB separated) output

	#seqID  start   end     island_len      score   quadruplex
	GeneID_1     947     997     3       26      CCC-agccccctccgggccct-CCC-agcccctc-CCC-ttcctttccgcggc-CCC
	GeneID_2     913     944     3       26      CCC-cgccccgt-CCC-gacccct-CCC-gggtc-CCC

Each line contains the sequence ID as in the fasta input, the start and the end indexes, the length of the islands, the score and the sequence for the PQS. The values are separated by TAB. In the PQS sequence the islands are represented in CAPS LOCK and are connected to loops by '-': 'ISL-loop-ISL-loop-ISL-loop-...-ISL'. Null loops are represented as 'ISL--ISL'.

######Output with alignments (-a)

	#seqID  start   end     island_len      score   quadruplex
	GeneID_1     947     997     3       26      CCC-agccccctccgggccct-CCC-agcccctc-CCC-ttcctttccgcggc-CCC
        Loop_1:
                agccccctc
                ||::||| :
                tcccggg-c
        Loop_2:
                -a-gc
                 | |:
                ctccc
        Loop_3:
                ttc-c-tt
                  | |  :
                cggcgcct
	GeneID_2     913     944     3       26      CCC-cgccccgt-CCC-gacccct-CCC-gggtc-CCC
        Loop_1:
                cgcc
                 :::
                tgcc
        Loop_2:
                gacc
                 |::
                -tcc
        Loop_3:
                --
                  
                --

When showing the alignment, in between each PQS (in tsv format) it is shown the optimal alignment for each of the loops. '|' represent a Watson-Crick pairing, ':' represent a Hoogsteen pairing. 
                ## **License**
Copyright (C) 2017 Michele Berselli

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

See http://www.gnu.org/licenses/ for more informations.