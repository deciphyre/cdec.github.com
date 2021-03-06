---
layout: documentation
title: SCFG Grammar extraction
---
SCFG grammar extraction (also called grammar induction, grammar learning, rule learning, and phrase table extraction), is the process of building a [SCFG translation grammar](/concepts/scfgs.html), usually from a [word-aligned](/concepts/alignment.html) parallel corpus. Note: there are many types of translation models with different formal characteristics; these instructions describe how to extract a "Hiero" grammar (an SCFG with a single nonterminal category).

The `cdec` distribution includes an online grammar extractor originally developed by [Adam Lopez](http://www.cs.jhu.edu/~alopez/) that is based on [suffix arrays](http://en.wikipedia.org/wiki/Suffix_array) for rapidly looking up patterns in a parallel corpus.

## Per-sentence translation grammars

Since `cdec` does not support an on-disk representation for fast access to grammars that are too large to fit into memory, it is strongly recommended that you use [per-sentence grammars](psgs.html) in translation. PSGs are filtered from larger grammars so as to only contain rules to match a single sentence. Although these must be created for each sentence that is to be translated, they are small enough to be loadable very quickly from the [human-readable grammar format](grammar-format.html).

## Online translation grammar extraction

Per-sentence grammars can be created directly using the tools in the `cdec.sa` Python package.

### Compile the training data

Use the tool `cdec.sa.compile` to compile a parallel corpus and word alignment into a suffix array that can then be used to quickly extract per-sentence grammars.

#### Command line options

 * `-c FILE.INI` - writes the configuration file to `FILE.INI`
 * `-b BITEXT.TXT` - the parallel data (one parallel sentence per line, separated by `|||`)
 * `-a ALIGNMENT.TXT` - the word alignment file (same number of lines as `BITEXT.TXT`)

### Extract grammars

The tool `cdec.sa.extract` generates per sentence grammars directly from the compiled parallel corpus. It reads sentences from standard in (one per line) and generates grammars in the specified directory. It writes the SGML marked up sentences that include pointers to the generated grammar to standard out. These can be read directly by the `cdec` decoder.

#### Command line options

 * `-c FILE.INI` - configuration file generated by `cdec.sa.compile`
 * `-g DIR/` - directory where the per-sentence grammars will be written
 * `-z` - gzip grammars. This is *strongly recommended* since the cdec grammar can read gzipped files.
 * `-j N` - run `N` processes in parallel

