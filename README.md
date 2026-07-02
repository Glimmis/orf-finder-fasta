# orf-finder-fasta

A small, notebook-friendly **Open Reading Frame (ORF) finder** for FASTA files.

It reads a FASTA file, scans every sequence on both the forward strand and the
reverse complement, translates each ORF to protein, and exports the results to a
CSV file. It deliberately avoids `input()` and `argparse` so it runs cleanly
inside a Jupyter Notebook without hanging.

## What it does

- Parses a FASTA file (multiple sequences supported).
- Finds complete ORFs (start codon `AUG` → stop codon) in all three reading
  frames, on both the forward strand and the reverse complement.
- Translates each ORF to protein (1-letter and 3-letter codes).
- Reports ORF length, GC content, protein length, and an estimated molecular
  weight (with peptide-bond water loss subtracted).
- Flags the longest ORF **per sequence**.
- Exports everything to a CSV named after the input file.

## Requirements

- Python 3 (uses only the standard library — `csv` and `os`).

## Usage

By default the script runs on a small built-in example so you can try it
immediately:

```bash
python orf_finder_fasta.py
```

This creates `example_sequences.fasta`, analyzes it, prints the ORFs, and writes
`example_sequences_orf_results.csv`.

### Running it on your own FASTA file

Open `orf_finder_fasta.py` and edit the run section near the bottom:

```python
USE_EXAMPLE_FASTA = False
FASTA_FILE = "your_sequences.fasta"
OUTPUT_CSV = None   # or "custom_name.csv"
```

Put your FASTA file in the same folder and run the script again. The results CSV
is named after your input file unless you set `OUTPUT_CSV`.

## Output columns

The CSV contains one row per ORF with: `sequence_id`, `sequence_header`,
`strand`, `frame`, `start`, `end`, `orf_length_bp`, `gc_content`,
`protein_1_letter`, `protein_3_letter`, `protein_length_aa`,
`protein_molecular_weight`, `longest_orf`, `dna`, and `rna`.

## Notes

- The molecular-weight table uses approximate average amino-acid masses, so the
  reported weight is an estimate.
- Only the standard genetic code is used, and only `A/T/C/G` sequences are
  accepted (others are skipped with a message).
