# OpenReview (OR) configuration for EMNLP 2025 demo papers

## Applying for OR venue

First, one has to apply for a new venue and fill out the form. I just filled the application form as good as I could, most of the configuration will be changed later anyway. Do this very soon. 

After getting the venue accepted, we can adjust the setup. Our OR main URL is https://openreview.net/group?id=EMNLP/2025/System_Demonstrations/

## Configuring OR

We inherited the OR configuration from EMNLP 2024 demo, available as a text-file in `emnlp24-config-export.txt`.

This file is basically a copy-pasted content from the browser from the OR "Full Venue Configuration" page. It was created partly by filling out some forms in OR and partly by inserting some JSON configuration to particular fields. We will have to do the same for EMNLP 2025. 

## Where to configure the venue

* Go to https://openreview.net/group?id=EMNLP/2025/System_Demonstrations/Program_Chairs
* Scroll down and click on "Full Venue Configuration"  https://openreview.net/forum?id=jjseaY5yN6&referrer=%5BProgram%20Chair%20Console%5D(%2Fgroup%3Fid%3DEMNLP%2F2025%2FSystem_Demonstrations%2FProgram_Chairs)
* This leads to the following page (see the screenshot below)

![screenshot](img/screenshot1.jpg)

### Configuring the main features of the conference (aka. "Revision")

* Click on "Revision"
* Fill out this super long form accordingly (basically all the basic configuration of the venue)
  * I looked up most of the setup from EMNLP24 config in `emnlp24-config-export.txt` 
* The content of the `Additional Submission Options` field must be a valid JSON, so copy & paste the content of `additional-config-json-files/revision-additional-submission-options.json` file

After each configuration change, you will receive a confusing e-mail and a comment saying something like

![screenshot](img/screenshot5.jpg)

When I got this first time, I panicked as I thought that **we now actually finished the process of reviewing and that I screwed up the entire conference**. Nope, I think this should simply read **Review Stage Configuration Updated** instead. See https://github.com/openreview/openreview-web/issues/2353 

You can check the final submission form by

1) going to the OR homepage

![screenshot](img/screenshot9.jpg)

2) click on "EMNLP 2025 System Demonstrations"

![screenshot](img/screenshot10.jpg)

3) click on "EMNLP 2025 System Demonstration Submission"

![screenshot](img/screenshot11.jpg)

### Skipping "Bid Stage"

* We didn't do anything with bidding

### Configuring "Review Stage"

Similarly to the main "Revision" setup, we adjusted the "Review Stage" following the 2024 configuration. Note that all deadlines are GMT, so August 29 11:59 GMT should be August 28 Anywhere on Earth - at least I hope it will work like that :)

* Here's a screenshot for convenience

![screenshot](img/screenshot2.jpg)

* for the field `Additional Review Form Options`, copy & paste the content of `additional-config-json-files/review-stage-additional-review-form-options.json`
* Note that EMNLP 2025 Demo track does not feature Best papers

### Configuring "Meta Review Stage"

* The same procedure applies for the Meta Review Stage

![screenshot](img/screenshot3.jpg)

* JSON configuration for the `Additional Meta Review Form Options` field available in
`additional-config-json-files/meta-review-stage-additional-meta-review-form-options.json`

### Configuring "Decision Stage"

For this stage, there is no additional JSON

![screenshot](img/screenshot4.jpg)

* Bug (or a feature?)
  * There is no field to configure `Notify Authors: No, I will send the emails to the authors` (this was shown in the EMNLP 24 configuration, but not available in the Decision Stage form)
  * However, it appears in the full configuration automatically after submitting changes to "Decision Stage" form

### Skipping "Comment Stage"

* AFAIK, there's nothing to be configured as we don't use it

### Configuring "Submission Revision Stage" (which corresponds to Camera Ready configuration)

![screenshot](img/screenshot6.jpg)

* We disabled changing (adding, removing) authors
* Nothing needed for `Submission Revision Additional Options`, AFAIK

### Configuring "Post Submission" stage

Here I have no idea, when this stage is activated, so I left the default option

![screenshot](img/screenshot7.jpg)

### Skipping "Reviewer Registration" and "Area Chair Registration"

We invited reviewers + ACs directly (under "Full Venue Configuration", scroll down below the configuration listing, on the right hand side there are three buttons, one reads "Recruitment") so this configuration seems to serve no purpose.

### Skipping "Ethics Review Stage"

Looks like EMNLP 24 also didn't configure this stage

## How to run the show - paper assignments, etc.

Items present in EMNLP 24 configuration which we are still missing:
* TODO

After all the configurations above (minus the missing items), the program chair console shows the following timeline:

![screenshot](img/screenshot8.jpg)

This gives me hope that we will be able to assign ACs to papers after the submission, so each AC can do desk rejects of their own batch.

However, after clicking on "Area Chairs Paper Assignment", we got:

> Error 404
> 
> The server responded with the following message:
>
> There is currently no assignment configuration ready for use. Please go to your venue request form and use the Paper Matching Setup to compute conflicts and/or affinity scores.

That's not a bug! According to the OR guidelines, **paper matching becomes available after the submission deadline**! See https://docs.openreview.net/how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-setup-paper-matching-by-calculating-affinity-scores-and-conflicts

### Enable Paper Matching (after submission deadline has passed)

* Go to "Full Venue Configuration"
* Scroll down, "Paper Matching Setup" should appear there

![screenshot](img/screenshot12.jpg)

* Click on "Paper Matching Setup" and fill out the form (I tried the following)

![screenshot](img/screenshot13.jpg)

**You have to wait at least 10 minutes after "Paper Matching Setup"** so that some scores are computed somewhere!!! After that, you will receive the usual e-mail notification (with some further info).

Now you can click on "Area Chair Paper Assignment". Loading the next page takes a while (like a loong time...)

![screenshot](img/screenshot14.jpg)

This is the starting point

![screenshot](img/screenshot15.jpg)

Let's do "New Assignment Configuration", which opens this horrifying pop-up

![screenshot](img/screenshot16.jpg)

Some fields are clearly explained (but somewhere else, not in this popup, see https://docs.openreview.net/how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-run-a-paper-matching), but some are hard to decipher

* What is "Alternates"?

So I tried this and left the rest as default (btw: solver: MinMax which is the default fallback)

![screenshot](img/screenshot17.jpg)

which gives us this new configuration listed

![screenshot](img/screenshot18.jpg)

The documentation says 

> You can view, edit or copy the values you filled out in the matching form. When you are happy with your configuration, you should hit 'Run Matcher' and wait until its status is 'Complete'.

I don't know if I'm happy because I have no clue, so let's run "Run Matcher". After some while, the overview shows "No Solution":

![screenshot](img/screenshot19.jpg)

OK, I think I messed up something - obviously we need just one AC per paper, so maybe "User Demand" should be just 1 instead of 3? This did the trick!

![screenshot](img/screenshot20.jpg)

So let's explore "View Statistics"

![screenshot](img/screenshot21.jpg)

Maybe we should look at the details to take care of smaller loads etc.

Here the **manual** re-configuration is necessary - on can click on the bars to select papers or users with a certain number of assignments and then un-assign or assign other persons (this is done through the standard "Edge Browser" in OR, similar to reviewer assignment in area chairing). This takes time and care to cater for ACs max load, etc.

After some manual work, each paper has one AC, all max load requests (btw we didn't put them into OR, I didn't know how to do that easily, so I checked with our list of ACs) and max load of 8 papers.

![screenshot](img/screenshot22.jpg)

It's time to deploy the assignment (https://docs.openreview.net/how-to-guides/paper-matching-and-assignment/how-to-do-automatic-assignments/how-to-deploy-the-proposed-assignments)

> Be careful before deploying proposed assignments!

Yeah, whatever... How can I be careful? So I very carefully clicked on "Deploy Assignment". That should work. It said "Deployment started." and that's it?! After refreshing, there is this new option, though...

![screenshot](img/screenshot23.jpg)

Looks like it worked! Now under "Area Chair Status" we can see the assignments!

![screenshot](img/screenshot24.jpg)

## Playground for testing OR

(Eventually I didn't play around that much for lack of time, but for a bigger conference, I'd do so to get really familiar with the entire workflow well in advance)

* Following OR recommendation here: https://docs.openreview.net/how-to-guides/workflow/how-to-test-your-venue-workflow
* Created a new account on https://dev.openreview.net
* Created EMNLP25 Demo Test venue and e-mailed OR to deploy, so we can test with multiple other accounts how the submission works, etc.

