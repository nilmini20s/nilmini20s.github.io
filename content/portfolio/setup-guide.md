---
title: "Setup Guide Extension: Accessing Test Results in Antithesis"
date: 2026-10-12
summary: "An extension to Antithesis's setup guide, adding sections to help users access and interpret their first test results."
image: "/images/card-setup.svg"
tags: ["How-To Guide"]
---

<div class="about-callout">

## About this writing sample

I wrote this as a writing exercise for Antithesis, a software testing company. The task was to extend their existing [Setup Guide](https://antithesis.com/docs/setup/docker_compose/) so it teaches readers how to access and interpret the results of their first test.

The existing guide ends once the user has kicked off a test run. My extension adds three new sections — Access your results, Understand your results, and Next steps — that orient the reader to the triage report, Logs Explorer, and multiverse map. I wrote the sections in a way that they could be appended to both the Docker Compose and the Kubernetes guides.

**Note:** Antithesis has since updated their setup guides. This sample was written against the earlier version of the guides, which configured test launches via curl rather than through the web app.

**Intended audience:** A developer who has just set up Antithesis and triggered their first test, and needs to find and interpret the results.

[View source on GitHub](https://github.com/nilmini20s/setup-guide-writing-sample)

</div>

*The sections below extend the existing Antithesis setup guide, picking up after step 7 ("Run your first test!").*

## Access your results

To check on your test's progress at any time, go to the **Recent runs** page in the web app.

Within an hour of launching your test, you'll receive an email with a link to a triage report. The email goes to the address you specified in `antithesis.report.recipients`. You can also get results through [Discord and Slack alerts](https://antithesis.com/docs/product/tooling_integrations/discord_slack/).

You can view the triage report in any of the following ways:
* Click the link in the email.
* Click the **Triage results** button next to the test on the **Recent runs** page.
* Click the **report url** link in the Discord or Slack alert.



## Understand your results

We suggest you read how to [understand your results](https://antithesis.com/docs/product/understanding_test_results/) while you wait for your test to finish! 

The [triage report](https://antithesis.com/docs/product/understanding_test_results/reports/) provides a high-level overview of your run's results. You can view an [interactive example triage report](https://antithesis.com/reports/example-triage-report). It contains four sections:
1. The **Environment** section lists container images and configuration used in the test.
2. The **Utilization** section gives you an overview of test length and behavior discovery.
3. The **Findings** section summarizes new, rare, and resolved findings. 
4. The **Properties** section shows pass/fail results for each of your test properties.

### Explore further with Logs Explorer

The [Logs Explorer and multiverse map](https://antithesis.com/docs/product/understanding_test_results/logs_explorer/) give you two more ways to explore your test results. 

The **Logs Explorer** lets you search for events across all the execution paths. Events stream in while the test is still running, so you can start searching before your test finishes.

You can view the Logs Explorer in any one of the following ways:
* Click the **Explore Logs** button on the triage report.
* Click the **Explore Logs** button on the **Recent runs** page.
* Go to the **Logs Explorer** page and select a run.

The **multiverse map** gives you a visual tree overview of all execution paths. You can view it by clicking the **Map** tab above the search results.



## Next steps

Congratulations — you're now set up with Antithesis!

From here, we suggest:

*   Reading about [how Antithesis works](https://antithesis.com/docs/introduction/how_antithesis_works) if you haven't done so yet, to understand some of the core concepts behind Antithesis testing.
*   Trying the [Test Composer tutorial](https://antithesis.com/docs/getting_started/tutorials/docker_compose/cluster-test/) for a ground-up guide to iterating on your testing.
*   Reading the [SDK docs](https://antithesis.com/docs/reference/sdk/) to integrate the relevant SDK with your code.

Later on, you'll probably also want to [configure your CI system](https://antithesis.com/docs/product/tooling_integrations/ci/) to automate the process of building your software and kicking off webhooks.
