# DOVER-Lap
Method for combining overlap-aware diarization system outputs.

## Prerequisites

We use RTTM processing code from the [dscore](https://github.com/nryant/dscore) repository to process the input RTTMs
into a list of turns. The codebase internally uses `intervaltree` package, which
can be installed as

```
python -m pip install -U intervaltree
```

## How to run

Clone this repository and then run the following script:

```
./run.py -i <input-RTTMs> -o <output-RTTM>
```

Example:

```
./run.py -i egs/ami/rttm_test_* -o egs/ami/rttm_dl_test
```

## Optional arguments

* **-u, --uem** 
: UEM file indicating scoring regions

* **-c, --channel**
: Channel ID for output RTTM (Default: 1)

* **--second-maximal**
: Boolean argument to specify whether to apply an additional round of maximal
matching in the label mapping stage. This may perform slightly better for larger
number of inputs (Default: False)

* **--dover-weight**
: Parameter for DOVER-style rank weighting applied to hypothesis for label
voting, e.g. w_k = (1/k)^0.1, where k is the rank (Default: 0.1)

## Results

We provide a sample result on the AMI mix-headset test set. The results can be 
obtained as follows:

```
./run.py -i egs/ami/rttm_test_* -o egs/ami/rttm_dl_test
./libs/md-eval.pl -r egs/ami/ref_rttm_test -s egs/ami/rttm_dl_test
```

and similarly for the input hypothesis. The DER results are shown below.

|                                   |   MS  |  FA  | Conf. |  DER  |
|-----------------------------------|:-----:|:----:|:-----:|:-----:|
| Overlap-aware VB resegmentation   |  9.84 | 2.06 |  9.60 | 21.50 |
| Overlap-aware spectral clustering | 11.48 | 2.27 |  9.81 | 23.56 |
| Region Proposal Network           |  **9.49** | 7.68 |  8.25 | 25.43 |
| DOVER-Lap                         | 10.66 | **2.03** |  **7.82** | **20.50** |


## Running time

The algorithm is implemented in pure Python with NumPy for tensor computations. 
The time complexity is expected to increase exponentially with the number of 
inputs, but it should be reasonable for combining up to 10 input hypotheses.

For smaller number of inputs (up to 5), the algorithm should take only a few seconds
to run on a laptop.

## Citation

Coming soon.

## Contact

For issues/bug reports, please raise an Issue in this repository, or reach out to me at `draj@cs.jhu.edu`.