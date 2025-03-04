# PTM model 


## Environment Setup

Set up exactly same environment as before!

## What is this model?

our released code and weight is an example of PrimeNovo finetuned on "Phosphorylation (+79.97)" data. 

This model is obtained by finetuning original PrimeNovo (trained with MassiveKB) on 2020-Cell-LUAD dataset, where training data is phosphorylation-enriched, so it's made for detecting phosphorylation PTM in some given data set. 

That's said, this model can predict 1 more token than original primenovo, for easy-processing, we have made such additional PTM using token "B", which is last token we added to config.yaml in this directory. 

When model is outputing its prediction, whenever there is a "B", it means it's "Phosphorylation (+79.97)" in this case.

But you can train/finetune your own PrimeNovo-PTM by changing this "B" to any other token you want, but your training data also has to change the target PTM to "B" so they matches during trianing.

If you want to changes this B to anything other than Phosphorylation (+79.97), here are all the places you need to make changes:

1. in `mass_con.py`: change variable `mass_b =79.9663` to whatever new PTM mole weight it has.

2. in `model.py`: change variable `mass_b =79.9663` to whatever new PTM mole weight it has.

3. in `transformers.py`: change variable `mass_b =79.9663` to whatever new PTM mole weight it has.

And that's it, you can then finetune on your own dataset with additional `1` PTM, just replace that PTM with letter "B" anywhere it appears 

## Run Instructions

**Note!!!!!!!!!!!!!!!!!!:** All the following steps should be performed under the main directory: `pi-PrimeNovo-PTM`. Do **not** use `cd ..` !!!!!!!!!!!!!!!!!!!

### Step 1: Download Required Files

To evaluate the provided test MGF file (you can replace this MGF file with your own), download the following files:

1. **Model Checkpoint**: [model_massive.ckpt](https://drive.google.com/file/d/12IZgeGP3ae3KksI5_82yuSTbk_M9sKNY/view?usp=share_link)
2. **Test MGF File**: [Bacillus.10k.mgf](https://drive.google.com/file/d/1HqfCETZLV9ZB-byU0pqNNRXbaPbTAceT/view?usp=drive_link)

**Note:** If you are using a remote server, you can use the `gdown` package to easily download the content from Google Drive to your server disk.

### Step 2: Choose the Mode

The `--mode` argument can be set to either:

- `eval`: Use this mode when evaluating data with a labeled dataset.
- `denovo`: Use this mode for de novo analysis on unlabeled data.

**Important**: Select `eval` only if your data is labeled.

### Step 3: Run the Commands

Execute the following command in the terminal:

```bash
python -m PrimeNovo.PrimeNovo --mode=eval --peak_path=./bacillus.10k.mgf --model=./model_massive.ckpt
```

This automatically uses all GPUs available in the current machine.

### Step 4: analyze the output

We include a sample running output ```./output.txt```. The performance for evaluation will be reported at the end of the output file.

If you are using ```denovo``` mode, you will get a ```denovo.tsv``` file under the current directory. The file has the following structure:

| label | prediction | charge | score |
| --- | --- | --- | --- |
| Title in MGF document | Sequence in [ProForma](https://doi.org/10.1021/acs.jproteome.1c00771) notation| Charge, as a number | Confidence score as number in range 0 and 1 using scientific notation |

The example below contains two peptides predicted based on some given spectrum:

```tsv
label	prediction	charge	score
MS_19321_2024_02_DDA	ATTALP	2	0.99
MS_19326_2024_02_DDA	TAM[+15.995]TR	2	0.87
```

## Citation

Bibtex：

```bibtex
@article{zhang2025pi,
  title={$\pi$-PrimeNovo: an accurate and efficient non-autoregressive deep learning model for de novo peptide sequencing},
  author={Zhang, Xiang and Ling, Tianze and Jin, Zhi and Xu, Sheng and Gao, Zhiqiang and Sun, Boyan and Qiu, Zijie and Wei, Jiaqi and Dong, Nanqing and Wang, Guangshuai and others},
  journal={Nature Communications},
  volume={16},
  number={1},
  pages={267},
  year={2025},
  publisher={Nature Publishing Group UK London}
}
```

Standard Citation:

```
Zhang, X., Ling, T., Jin, Z. et al. π-PrimeNovo: an accurate and efficient non-autoregressive deep learning model for de novo peptide sequencing. Nat Commun 16, 267 (2025). https://doi.org/10.1038/s41467-024-55021-3
```
