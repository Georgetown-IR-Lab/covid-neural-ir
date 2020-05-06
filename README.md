# SLEDGE: A Simple Yet Effective Baseline for Coronavirus Scientific Knowledge Search

Below are the steps to reproduce the primary results from Sean MacAvaney, Arman Cohan, and Nazli Goharian. 2020. SLEDGE: A Simple Yet Effective Baseline for  Coronavirus Scientific Knowledge Search. arXiv. [pdf](https://arxiv.org/pdf/2005.02365.pdf)

The code to reproduce the paper is incorporated into [OpenNIR](https://github.com/Georgetown-IR-Lab/OpenNIR).
Refer to https://opennir.net/ go get started.

Note: Despite our best efforts to control for differences, results may vary based on hardware configurations and other seemingly random conditions. Using a Quadro RTX 8000, we were able to get within 0.02 nDCG@10 and within 0.01 P@5 / P@5 (rel2) of our submitted runs (97% judged@5 rate).

## Step 1 of 1: Train and Evaluate SLEDGE

Training and validation on MS-MARCO (domain transfer setting):

```bash
$ scripts/pipeline.sh config/sledge
# (will prompt to download MS-MARCO data the first time -- it takes some time to download, process, index, etc.)
[snip]
valid epoch=50 map=0.2843 [mrr@10=0.2933] ndcg@20=0.3296 p@1=0.2097 rprec=0.2056
```

When you're ready to evaluate on the COVID-19 collection, provide `pipeline.test=True`:

```bash
$ scripts/pipeline.sh config/sledge pipeline.test=True
# (will prompt to download CORD-19 and TREC-COVID data the first time -- may take ~40 minutes for downloading, processing, indexing, etc.)
[snip]
test  epoch=50 judged@5=0.9667 ndcg@10=0.6661 p@5=0.7800 p_rel-2@5=0.6467
```

If you want to use for round 2, configure your system by:

 - setting `test_ds.date=2020-05-01` to use the May 1, 2020 release of CORD-19
 - setting `test_ds.subset=rnd2-query`, `test_ds.subset=rnd2-quest`, `test_ds.subset=rnd2-narr` use the queries, questions, narratives for Round 2 topics.
 - resetting `test_ds.bs_override=`

If you want to train on the medical-related subset of MS-MARCO, configure your system by:

 - setting `train_ds.subset=train_med`

## Citation

If you use this work, please cite:

```bibtex
@article{macavaney:arxiv2020-sledge,
  author = {MacAvaney, Sean and Cohan, Arman and Goharian, Nazli},
  title = {SLEDGE: A Simple Yet Effective Baseline for Coronavirus Scientific Knowledge Search},
  year = {2020},
  journal = {arXiv},
  volume = {abs/2005.02365}
}
```

Last Updated: May 6, 2020
