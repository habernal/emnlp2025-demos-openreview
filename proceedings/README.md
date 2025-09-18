# How we built the EMNLP demo proceedings

(based on the latest version of aclpub2 fetched from Github, and my previous notes for building PrivateNLP workshop at NAACL 2025)

```plaintext
$ virtualenv venv
$ source venv/bin/activate
$ pip install --upgrade pip
$ pip install -r requirements.txt
```

## Download papers from OpenReview

```plaintext
$ python openreview/or2papers.py ivan.habernal@ruhr-uni-bochum.de my_OR_pass EMNLP/2025/System_Demonstrations --all --pdfs
```

Console output

```plain
Paper zwvOv0fNt1 track field is not present. Contact info@openreview.net if you need this information migrated from ARR
paper_type field (long or short) is not present. Contact info@openreview.net if you need this information migrated from ARR
ERROR: or_id not found aaa@uuu.com
ERROR: or_id not found aaa@uuu.com
(and so on for the authors)
```

Didn't fail! :) This downloads all 77 accepted papers only to `papers` and `papers.yml`

## Download PCs from OR

```plaintext
$ python openreview/or2program_committee.py ivan.habernal@ruhr-uni-bochum.de my_OR_pass EMNLP/2025/System_Demonstrations
```

downloads `program_committee.yml` which should be copied and edited to the demo folder (see below)

## Create a new folder in `examples`

* I copied `privatenlp25ws` to `emnlp25demos` first (without PDFs in `papers`) to have some baseline to adjust
* Copy into it the `papers` folder with all 77 PDFs from the previous step
* Copy the `papers.yml` + `program_committee.yml` files too

## Generate the proceedings TODO continue

$ rm -rf ./build/ ; rm -rf ./output/; export PYTHONPATH=.; python bin/generate examples/privatenlp25ws --proceedings --overwrite

It generates the full proceedings PDF in `output`

## Fixing individual papers, round 1

TBD

## ACL pub check

Optional; Update aclpubcheck

```plain
cd ~/PycharmProjects/aclpubcheck
git pull
source venv/bin/activate
pip install
```

Run the check on all papers

(venv) ~/PycharmProjects/aclpubcheck$ for i in ~/PycharmProjects/privatenlp25-proceedings/examples/privatenlp25ws/papers/*.pdf ; do echo ${i}; ./venv/bin/aclpubcheck --paper_type long ${i} ; done > errors.log.txt











## Old notes, maybe useful later

PrivateNLP workshop: ACLPubCheck errors in the camera ready PDF

Dear authors,

Thanks again for submitting your camera-ready PDF for the PrivateNLP workshop.

Unfortunately when compiling the final workshop proceedings, I checked the submissions with the required ACLPubCheck tool and found some violations; see below. Can you please fix that and send me the PDF back?

Many thanks,

Ivan



## Filling out the Google sheet with authors and papers

In-house script: https://github.com/rycolab/aclpub2/pull/184

Run $ python bin/papers2sheet.py