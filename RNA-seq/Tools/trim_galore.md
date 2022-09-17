 trim_galore --help

 USAGE:

trim_galore [options] <filename(s)>

```
-h/--help               Print this help message and exits.

-v/--version            Print the version information and exits.

-q/--quality <INT>      Trim low-quality ends from reads in addition to adapter removal. For
                        RRBS samples, quality trimming will be performed first, and adapter
                        trimming is carried in a second round. Other files are quality and adapter
                        trimmed in a single pass. The algorithm is the same as the one used by BWA
                        (Subtract INT from all qualities; compute partial sums from all indices
                        to the end of the sequence; cut sequence at the index at which the sum is
                        minimal). Default Phred score: 20.
  除了移除adapter之外，还可以从读取中修剪低质量末端。 对于RRBS 样本，将首先执行质量修整，然后adapter修剪在第二轮中进行。 其他文件是质量修整和adapter一次修剪。 
  该算法与 BWA 使用的算法相同（从所有质量中减去 INT；从所有索引中计算部分和到序列的末尾； 在总和所在的索引处切割序列最小）。

--phred33               Instructs Cutadapt to use ASCII+33 quality scores as Phred scores指示 Cutadapt 使用 ASCII+33 质量分数作为 Phred 分数
                        (Sanger/Illumina 1.9+ encoding) for quality trimming. Default: ON.

--phred64               Instructs Cutadapt to use ASCII+64 quality scores as Phred scores
                        (Illumina 1.5 encoding) for quality trimming.

--fastqc                Run FastQC in the default mode on the FastQ file once trimming is complete.trim后接着质控

--fastqc_args "<ARGS>"  Passes extra arguments to FastQC. If more than one argument is to be passed
                        to FastQC they must be in the form "arg1 arg2 etc.". An example would be:
                        --fastqc_args "--nogroup --outdir /home/". Passing extra arguments will
                        automatically invoke FastQC, so --fastqc does not have to be specified
                        separately.

-a/--adapter <STRING>   Adapter sequence to be trimmed. If not specified explicitly, Trim Galore will
                        try to auto-detect whether the Illumina universal, Nextera transposase or Illumina
                        small RNA adapter sequence was used. Also see '--illumina', '--nextera' and
                        '--small_rna'. If no adapter can be detected within the first 1 million sequences
                        of the first file specified or if there is a tie between several adapter sequences,
                        Trim Galore defaults to '--illumina' (as long as the Illumina adapter was one of the
                        options, else '--nextera' is the default). A single base
                        may also be given as e.g. -a A{10}, to be expanded to -a AAAAAAAAAA.
  要修剪的adapter序列。  如果未明确指定，Trim Galore 将尝试自动检测是否使用了 Illumina 通用接头序列、Nextera 转座酶接头序列或 Illumina 小 RNA 接头序列。 另见“--illumina”、“--nextera”和“--small_rna”。 
  如果在指定的第一个文件的前 100 万个序列中没有检测到adapter，或者如果几个adapter序列之间存在关联，
  则 Trim Galore 默认为“--illumina”（只要 Illumina 适配器是选项之一， 否则 '--nextera' 是默认值）。。

-a2/--adapter2 <STRING> Optional adapter sequence to be trimmed off read 2 of paired-end files. This
                        option requires '--paired' to be specified as well. If the libraries to be trimmed
                        are smallRNA then a2 will be set to the Illumina small RNA 5' adapter automatically
                        (GATCGTCGGACT). A single base may also be given as e.g. -a2 A{10}, to be expanded
                        to -a2 AAAAAAAAAA.
  可选的adapter序列被修剪掉read 2 的双末端文件。 此选项还需要指定“--paired”。 如果要修剪的文库是 smallRNA，则 a2 将自动设置为 Illumina small RNA 5' adapter (GATCGTCGGACT)。

--illumina              Adapter sequence to be trimmed is the first 13bp of the Illumina universal adapter
                        'AGATCGGAAGAGC' instead of the default auto-detection of adapter sequence.
  要修剪的接头序列是 Illumina 通用接头“AGATCGGAAGAGC”的前 13bp，而不是接头序列的默认自动检测。

--stranded_illumina     Adapter sequence to be trimmed is the first 13bp of the Illumina stranded mRNA or Total
                        RNA adapter 'ACTGTCTCTTATA' instead of the default auto-detection of adapter sequence.
                        Note that this sequence resembles the Nextera sequence with an additional A from A-tailing.
                        Please also see https://github.com/FelixKrueger/TrimGalore/issues/127 or
                        https://support.illumina.com/bulletins/2020/06/trimming-t-overhang-options-for-the-illumina-rna-library-prep-wo.html
                        for further information. This sequence is currently NOT included in the adapter auto-detection.

--nextera               Adapter sequence to be trimmed is the first 12bp of the Nextera adapter
                        'CTGTCTCTTATA' instead of the default auto-detection of adapter sequence.

--small_rna             Adapter sequence to be trimmed is the first 12bp of the Illumina Small RNA 3' Adapter
                        'TGGAATTCTCGG' instead of the default auto-detection of adapter sequence. Selecting
                        to trim smallRNA adapters will also lower the --length value to 18bp. If the smallRNA
                        libraries are paired-end then a2 will be set to the Illumina small RNA 5' adapter
                        automatically (GATCGTCGGACT) unless -a 2 had been defined explicitly.

--consider_already_trimmed <INT>     During adapter auto-detection, the limit set by <INT> allows the user to
                        set a threshold up to which the file is considered already adapter-trimmed. If no adapter
                        sequence exceeds this threshold, no additional adapter trimming will be performed (technically,
                        the adapter is set to '-a X'). Quality trimming is still performed as usual.
                        Default: NOT SELECTED (i.e. normal auto-detection precedence rules apply).

--max_length <INT>      Discard reads that are longer than <INT> bp after trimming. This is only advised for
                        smallRNA sequencing to remove non-small RNA sequences.


--stringency <INT>      Overlap with adapter sequence required to trim a sequence. Defaults to a
                        very stringent setting of 1, i.e. even a single bp of overlapping sequence
                        will be trimmed off from the 3' end of any read.

-e <ERROR RATE>         Maximum allowed error rate (no. of errors divided by the length of the matching
                        region) (default: 0.1)

--gzip                  Compress the output file with GZIP. If the input files are GZIP-compressed
                        the output files will automatically be GZIP compressed as well. As of v0.2.8 the
                        compression will take place on the fly.

--dont_gzip             Output files won't be compressed with GZIP. This option overrides --gzip.

--length <INT>          Discard reads that became shorter than length INT because of either
                        quality or adapter trimming. A value of '0' effectively disables
                        this behaviour. Default: 20 bp.

                        For paired-end files, both reads of a read-pair need to be longer than
                        <INT> bp to be printed out to validated paired-end files (see option --paired).
                        If only one read became too short there is the possibility of keeping such
                        unpaired single-end reads (see --retain_unpaired). Default pair-cutoff: 20 bp.

--max_n COUNT           The total number of Ns (as integer) a read may contain before it will be removed altogether.
                        In a paired-end setting, either read exceeding this limit will result in the entire
                        pair being removed from the trimmed output files.

--trim-n                Removes Ns from either side of the read. This option does currently not work in RRBS mode.

-o/--output_dir <DIR>   If specified all output will be written to this directory instead of the current
                        directory. If the directory doesn't exist it will be created for you.

--no_report_file        If specified no report file will be generated.

--suppress_warn         If specified any output to STDOUT or STDERR will be suppressed.

--clip_R1 <int>         Instructs Trim Galore to remove <int> bp from the 5' end of read 1 (or single-end
                        reads). This may be useful if the qualities were very poor, or if there is some
                        sort of unwanted bias at the 5' end. Default: OFF.

--clip_R2 <int>         Instructs Trim Galore to remove <int> bp from the 5' end of read 2 (paired-end reads
                        only). This may be useful if the qualities were very poor, or if there is some sort
                        of unwanted bias at the 5' end. For paired-end BS-Seq, it is recommended to remove
                        the first few bp because the end-repair reaction may introduce a bias towards low
                        methylation. Please refer to the M-bias plot section in the Bismark User Guide for
                        some examples. Default: OFF.

--three_prime_clip_R1 <int>     Instructs Trim Galore to remove <int> bp from the 3' end of read 1 (or single-end
                        reads) AFTER adapter/quality trimming has been performed. This may remove some unwanted
                        bias from the 3' end that is not directly related to adapter sequence or basecall quality.
                        Default: OFF.

--three_prime_clip_R2 <int>     Instructs Trim Galore to remove <int> bp from the 3' end of read 2 AFTER
                        adapter/quality trimming has been performed. This may remove some unwanted bias from
                        the 3' end that is not directly related to adapter sequence or basecall quality.
                        Default: OFF.

--2colour/--nextseq INT This enables the option '--nextseq-trim=3'CUTOFF' within Cutadapt, which will set a quality
                        cutoff (that is normally given with -q instead), but qualities of G bases are ignored.
                        This trimming is in common for the NextSeq- and NovaSeq-platforms, where basecalls without
                        any signal are called as high-quality G bases. This is mutually exlusive with '-q INT'.


--path_to_cutadapt </path/to/cutadapt>     You may use this option to specify a path to the Cutadapt executable,
                        e.g. /my/home/cutadapt-1.7.1/bin/cutadapt. Else it is assumed that Cutadapt is in
                        the PATH.

--basename <PREFERRED_NAME>     Use PREFERRED_NAME as the basename for output files, instead of deriving the filenames from
                        the input files. Single-end data would be called PREFERRED_NAME_trimmed.fq(.gz), or
                        PREFERRED_NAME_val_1.fq(.gz) and PREFERRED_NAME_val_2.fq(.gz) for paired-end data. --basename
                        only works when 1 file (single-end) or 2 files (paired-end) are specified, but not for longer lists.

-j/--cores INT          Number of cores to be used for trimming [default: 1]. For Cutadapt to work with multiple cores, it
                        requires Python 3 as well as parallel gzip (pigz) installed on the system. Trim Galore attempts to detect
                        the version of Python used by calling Cutadapt. If Python 2 is detected, --cores is set to 1. If the Python
                        version cannot be detected, Python 3 is assumed and we let Cutadapt handle potential issues itself.

                        If pigz cannot be detected on your system, Trim Galore reverts to using gzip compression. Please note
                        that gzip compression will slow down multi-core processes so much that it is hardly worthwhile, please
                        see: https://github.com/FelixKrueger/TrimGalore/issues/16#issuecomment-458557103 for more info).

                        Actual core usage: It should be mentioned that the actual number of cores used is a little convoluted.
                        Assuming that Python 3 is used and pigz is installed, --cores 2 would use 2 cores to read the input
                        (probably not at a high usage though), 2 cores to write to the output (at moderately high usage), and
                        2 cores for Cutadapt itself + 2 additional cores for Cutadapt (not sure what they are used for) + 1 core
                        for Trim Galore itself. So this can be up to 9 cores, even though most of them won't be used at 100% for
                        most of the time. Paired-end processing uses twice as many cores for the validation (= writing out) step.
                        --cores 4 would then be: 4 (read) + 4 (write) + 4 (Cutadapt) + 2 (extra Cutadapt) +     1 (Trim Galore) = 15.

                        It seems that --cores 4 could be a sweet spot, anything above has diminishing returns.
```


SPECIFIC TRIMMING - without adapter/quality trimming
```
--hardtrim5 <int>       Instead of performing adapter-/quality trimming, this option will simply hard-trim sequences
                        to <int> bp at the 5'-end. Once hard-trimming of files is complete, Trim Galore will exit.
                        Hard-trimmed output files will end in .<int>_5prime.fq(.gz). Here is an example:

                        before:         CCTAAGGAAACAAGTACACTCCACACATGCATAAAGGAAATCAAATGTTATTTTTAAGAAAATGGAAAAT
                        --hardtrim5 20: CCTAAGGAAACAAGTACACT

--hardtrim3 <int>       Instead of performing adapter-/quality trimming, this option will simply hard-trim sequences
                        to <int> bp at the 3'-end. Once hard-trimming of files is complete, Trim Galore will exit.
                        Hard-trimmed output files will end in .<int>_3prime.fq(.gz). Here is an example:

                        before:         CCTAAGGAAACAAGTACACTCCACACATGCATAAAGGAAATCAAATGTTATTTTTAAGAAAATGGAAAAT
                        --hardtrim3 20:                                                   TTTTTAAGAAAATGGAAAAT

--clock                 In this mode, reads are trimmed in a specific way that is currently used for the Mouse
                        Epigenetic Clock (see here: Multi-tissue DNA methylation age predictor in mouse, Stubbs et al.,
                        Genome Biology, 2017 18:68 https://doi.org/10.1186/s13059-017-1203-5). Following this, Trim Galore
                        will exit.

                        In it's current implementation, the dual-UMI RRBS reads come in the following format:

                        Read 1  5' UUUUUUUU CAGTA FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF TACTG UUUUUUUU 3'
                        Read 2  3' UUUUUUUU GTCAT FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF ATGAC UUUUUUUU 5'

                        Where UUUUUUUU is a random 8-mer unique molecular identifier (UMI), CAGTA is a constant region,
                        and FFFFFFF... is the actual RRBS-Fragment to be sequenced. The UMIs for Read 1 (R1) and
                        Read 2 (R2), as well as the fixed sequences (F1 or F2), are written into the read ID and
                        removed from the actual sequence. Here is an example:

                        R1: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 1:N:0: CGATGTTT
                            ATCTAGTTCAGTACGGTGTTTTCGAATTAGAAAAATATGTATAGAGGAAATAGATATAAAGGCGTATTCGTTATTG
                        R2: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 3:N:0: CGATGTTT
                            CAATTTTGCAGTACAAAAATAATACCTCCTCTATTTATCCAAAATCACAAAAAACCACCCACTTAACTTTCCCTAA

                        R1: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 1:N:0: CGATGTTT:R1:ATCTAGTT:R2:CAATTTTG:F1:CAGT:F2:CAGT
                                         CGGTGTTTTCGAATTAGAAAAATATGTATAGAGGAAATAGATATAAAGGCGTATTCGTTATTG
                        R2: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 3:N:0: CGATGTTT:R1:ATCTAGTT:R2:CAATTTTG:F1:CAGT:F2:CAGT
                                         CAAAAATAATACCTCCTCTATTTATCCAAAATCACAAAAAACCACCCACTTAACTTTCCCTAA

                        Following clock trimming, the resulting files (.clock_UMI.R1.fq(.gz) and .clock_UMI.R2.fq(.gz))
                        should be adapter- and quality trimmed with Trim Galore as usual. In addition, reads need to be trimmed
                        by 15bp from their 3' end to get rid of potential UMI and fixed sequences. The command is:

                        trim_galore --paired --three_prime_clip_R1 15 --three_prime_clip_R2 15 *.clock_UMI.R1.fq.gz *.clock_UMI.R2.fq.gz

                        Following this, reads should be aligned with Bismark and deduplicated with UmiBam
                        in '--dual_index' mode (see here: https://github.com/FelixKrueger/Umi-Grinder). UmiBam recognises
                        the UMIs within this pattern: R1:(ATCTAGTT):R2:(CAATTTTG): as (UMI R1) and (UMI R2).

--polyA                 This is a new, still experimental, trimming mode to identify and remove poly-A tails from sequences.
                        When --polyA is selected, Trim Galore attempts to identify from the first supplied sample whether
                        sequences contain more often a stretch of either 'AAAAAAAAAA' or 'TTTTTTTTTT'. This determines
                        if Read 1 of a paired-end end file, or single-end files, are trimmed for PolyA or PolyT. In case of
                        paired-end sequencing, Read2 is trimmed for the complementary base from the start of the reads. The
                        auto-detection uses a default of A{20} for Read1 (3'-end trimming) and T{150} for Read2 (5'-end trimming).
                        These values may be changed manually using the options -a and -a2.

                        In addition to trimming the sequences, white spaces are replaced with _ and it records in the read ID
                        how many bases were trimmed so it can later be used to identify PolyA trimmed sequences. This is currently done
                        by writing tags to both the start ("32:A:") and end ("_PolyA:32") of the reads in the following example:

                        @READ-ID:1:1102:22039:36996 1:N:0:CCTAATCC
                        GCCTAAGGAAACAAGTACACTCCACACATGCATAAAGGAAATCAAATGTTATTTTTAAGAAAATGGAAAATAAAAACTTTATAAACACCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA

                        @32:A:READ-ID:1:1102:22039:36996_1:N:0:CCTAATCC_PolyA:32
                        GCCTAAGGAAACAAGTACACTCCACACATGCATAAAGGAAATCAAATGTTATTTTTAAGAAAATGGAAAATAAAAACTTTATAAACACC

                        PLEASE NOTE: The poly-A trimming mode expects that sequences were both adapter and quality trimmed
                        before looking for Poly-A tails, and it is the user's responsibility to carry out an initial round of
                        trimming. The following sequence:

                        1) trim_galore file.fastq.gz
                        2) trim_galore --polyA file_trimmed.fq.gz
                        3) zcat file_trimmed_trimmed.fq.gz | grep -A 3 PolyA | grep -v ^-- > PolyA_trimmed.fastq

                        Will 1) trim qualities and Illumina adapter contamination, 2) find and remove PolyA contamination.
                        Finally, if desired, 3) will specifically find PolyA trimmed sequences to a specific FastQ file of your choice.

--implicon              This is a special mode of operation for paired-end data, such as required for the IMPLICON method, where a UMI sequence
                        is getting transferred from the start of Read 2 to the readID of both reads. Following this, Trim Galore will exit.

                        In it's current implementation, the UMI carrying reads come in the following format:

                        Read 1  5' FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF 3'
                        Read 2  3' UUUUUUUUFFFFFFFFFFFFFFFFFFFFFFFFFFFF 5'

                        Where UUUUUUUU is a random 8-mer unique molecular identifier (UMI) and FFFFFFF... is the actual fragment to be
                        sequenced. The UMI of Read 2 (R2) is written into the read ID of both reads and removed from the actual sequence.
                        Here is an example:

                        R1: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 1:N:0: CGATGTTT
                            ATCTAGTTCAGTACGGTGTTTTCGAATTAGAAAAATATGTATAGAGGAAATAGATATAAAGGCGTATTCGTTATTG
                        R2: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 3:N:0: CGATGTTT
                            CAATTTTGCAGTACAAAAATAATACCTCCTCTATTTATCCAAAATCACAAAAAACCACCCACTTAACTTTCCCTAA

                        After --implicon trimming:
                        R1: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 1:N:0: CGATGTTT:CAATTTTG
                            ATCTAGTTCAGTACGGTGTTTTCGAATTAGAAAAATATGTATAGAGGAAATAGATATAAAGGCGTATTCGTTATTG
                        R2: @HWI-D00436:407:CCAETANXX:1:1101:4105:1905 3:N:0: CGATGTTT:CAATTTTG
                                    CAGTACAAAAATAATACCTCCTCTATTTATCCAAAATCACAAAAAACCACCCACTTAACTTTCCCTAA
```
RRBS-specific options (MspI digested material):
```
--rrbs                  Specifies that the input file was an MspI digested RRBS sample (recognition
                        site: CCGG). Single-end or Read 1 sequences (paired-end) which were adapter-trimmed
                        will have a further 2 bp removed from their 3' end. Sequences which were merely
                        trimmed because of poor quality will not be shortened further. Read 2 of paired-end
                        libraries will in addition have the first 2 bp removed from the 5' end (by setting
                        '--clip_r2 2'). This is to avoid using artificial methylation calls from the filled-in
                        cytosine positions close to the 3' MspI site in sequenced fragments.
                        This option is not recommended for users of the NuGEN ovation RRBS System 1-16
                        kit (see below).

--non_directional       Selecting this option for non-directional RRBS libraries will screen
                        quality-trimmed sequences for 'CAA' or 'CGA' at the start of the read
                        and, if found, removes the first two basepairs. Like with the option
                        '--rrbs' this avoids using cytosine positions that were filled-in
                        during the end-repair step. '--non_directional' requires '--rrbs' to
                        be specified as well. Note that this option does not set '--clip_r2 2' in
                        paired-end mode.

--keep                  Keep the quality trimmed intermediate file. Default: off, which means
                        the temporary file is being deleted after adapter trimming. Only has
                        an effect for RRBS samples since other FastQ files are not trimmed
                        for poor qualities separately.
```

Note for RRBS using the NuGEN Ovation RRBS System 1-16 kit:

Owing to the fact that the NuGEN Ovation kit attaches a varying number of nucleotides (0-3) after each MspI
site Trim Galore should be run WITHOUT the option --rrbs. This trimming is accomplished in a subsequent
diversity trimming step afterwards (see their manual).



Note for RRBS using MseI:

If your DNA material was digested with MseI (recognition motif: TTAA) instead of MspI it is NOT necessary
to specify --rrbs or --non_directional since virtually all reads should start with the sequence
'TAA', and this holds true for both directional and non-directional libraries. As the end-repair of 'TAA'
restricted sites does not involve any cytosines it does not need to be treated especially. Instead, simply
run Trim Galore! in the standard (i.e. non-RRBS) mode.




Paired-end specific options:
```
--paired                This option performs length trimming of quality/adapter/RRBS trimmed reads for
                        paired-end files. To pass the validation test, both sequences of a sequence pair
                        are required to have a certain minimum length which is governed by the option
                        --length (see above). If only one read passes this length threshold the
                        other read can be rescued (see option --retain_unpaired). Using this option lets
                        you discard too short read pairs without disturbing the sequence-by-sequence order
                        of FastQ files which is required by many aligners.

                        Trim Galore! expects paired-end files to be supplied in a pairwise fashion, e.g.
                        file1_1.fq file1_2.fq SRR2_1.fq.gz SRR2_2.fq.gz ... .

-t/--trim1              Trims 1 bp off every read from its 3' end. This may be needed for FastQ files that
                        are to be aligned as paired-end data with Bowtie. This is because Bowtie (1) regards
                        alignments like this:

                          R1 --------------------------->     or this:    ----------------------->  R1
                          R2 <---------------------------                       <-----------------  R2

                        as invalid (whenever a start/end coordinate is contained within the other read).
                        NOTE: If you are planning to use Bowtie2, BWA etc. you don't need to specify this option.

--retain_unpaired       If only one of the two paired-end reads became too short, the longer
                        read will be written to either '.unpaired_1.fq' or '.unpaired_2.fq'
                        output files. The length cutoff for unpaired single-end reads is
                        governed by the parameters -r1/--length_1 and -r2/--length_2. Default: OFF.

-r1/--length_1 <INT>    Unpaired single-end read length cutoff needed for read 1 to be written to
                        '.unpaired_1.fq' output file. These reads may be mapped in single-end mode.
                        Default: 35 bp.

-r2/--length_2 <INT>    Unpaired single-end read length cutoff needed for read 2 to be written to
                        '.unpaired_2.fq' output file. These reads may be mapped in single-end mode.
                        Default: 35 bp.
```
Last modified on 15 January 2022.
