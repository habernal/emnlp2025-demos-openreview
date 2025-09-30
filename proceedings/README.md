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

### Compress excessive bitmap graphics in PDFs

300 dpi max

```plaintext
cd papers
for i in *.pdf ; do fname=$(basename "$i" .pdf); gs -sDEVICE=pdfwrite -dCombapibilityLevel=1.4 -dPDFSETTINGS=/printer -dColorImageResolution=300 -dNOPAUSE -dQUIET -dBATCH -sOutputFile="${fname}_comp.pdf" "${i}"; rm "${i}"; mv "${fname}_comp.pdf" "${i}"; done
```

Final size 81 MB instead of 205 MB orig


## Generate the proceedings

```plaintext
$ rm -rf ./build/ ; rm -rf ./output/; export PYTHONPATH=.; python bin/generate examples/emnlp25demos --proceedings --overwrite
```

It generates the full proceedings PDF in `output` (takes a while for 77 papers...)

## Fixing individual papers, round 1

all done by e-mailing them; I always uploaded the final camera ready version to OR so there is only one source of truth

139 should be compressed - 10 MB :/

## Fixing authors info missing in OR

Needs to be fixed manually in papers.yml -- look at the paper PDF, find the author who might have the given e-mail, and fix first name, last name, name, and institution





## ACL pub check

Install pub-check
```plaintext
cd ~/PycharmProjects
git clone https://github.com/acl-org/aclpubcheck.git
virtualenv venv
```
or update
```plain
cd ~/PycharmProjects/aclpubcheck
git pull
```
```plaintext
source venv/bin/activate
pip install .
```

Now here's a one-liner that checks all PDFs and outputs a CSV with `id,number_of_errors_detected`. If the paper is OK, the number of errors is zero. It takes a while to finish for 77 papers.

```plaintext
echo "id,errors" > results.csv; for i in $(ls ~/PycharmProjects/emnlp2025-demos-openreview/proceedings/papers/*.pdf | sort); do out=$(./venv/bin/aclpubcheck --paper_type long "$i" 2>&1); fname=$(basename "$i" .pdf); if [[ $out == *"All Clear!"* ]]; then echo "$fname,0"; elif [[ $out =~ We\ detected\ ([0-9]+)\ errors ]]; then echo "$fname,${BASH_REMATCH[1]}"; elif [[ $out =~ Error ]]; then echo "$fname,1"; else echo "$fname,\"$(echo "$out" | tr '\n' ' ' | sed 's/"/""/g')\""; fi; done >> results.csv
```

Double-check the number of papers, must be 77

```plaintext
$ wc -l < results.csv | awk '{print $1 - 1}'
```

How many papers have still errors?

```plaintext
awk -F, 'NR>1 && $2 != 0 {count++} END {print count}' results.csv 
```

Print IDs of papers with errors (sorted by ID)

```plaintext
awk -F, 'NR>1 && $2 != 0 {print $1}' results.csv | sort -n
```




## Pinging authors

54 - this is word, looks good


Many papers were submitted with page numbers...

They need just to change a single line to:

```plaintext
\usepackage[final]{acl}
```


PrivateNLP workshop: ACLPubCheck errors in the camera ready PDF

Dear authors,

Thanks again for submitting your camera-ready PDF for the PrivateNLP workshop.

Unfortunately when compiling the final workshop proceedings, I checked the submissions with the required ACLPubCheck tool and found some violations; see below. Can you please fix that and send me the PDF back?

Many thanks,

Ivan



## Filling out the Google sheet with authors and papers

In-house script: https://github.com/rycolab/aclpub2/pull/184

Run $ python bin/papers2sheet.py