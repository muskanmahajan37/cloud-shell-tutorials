# Dataflow Word Count Tutorial

<walkthrough-tutorial-url url="https://cloud.google.com/dataflow/docs/quickstarts/quickstart-python"></walkthrough-tutorial-url>
<walkthrough-watcher-constant key="directory" value="dataflow-intro"></walkthrough-watcher-constant>
<walkthrough-watcher-constant key="job-name" value="dataflow-intro"></walkthrough-watcher-constant>

## Introduction

In this tutorial, you'll learn the basics of the Cloud Dataflow service by
running a simple example pipeline using Python.

Dataflow pipelines are either *batch* (processing bounded input like a file or
database table) or *streaming* (processing unbounded input from a source like
Cloud Pub/Sub). The example in this tutorial is a batch pipeline that counts
words in a collection of Shakespeare's works.

Before you start, you'll need to check for prerequisites in your Cloud Platform
project and perform initial setup.

## Project setup

Google Cloud Platform organizes resources into projects. This allows you to
collect all the related resources for a single application in one place.

<walkthrough-project-billing-setup></walkthrough-project-billing-setup>
<walkthrough-project-permissions permissions="dataflow.jobs.create"></walkthrough-project-permissions>

## Set up Cloud Dataflow

To use Dataflow, turn on the Cloud Dataflow APIs and open the Cloud Shell.

### Turn on Google Cloud APIs

<walkthrough-enable-apis apis=
  "compute.googleapis.com,dataflow,cloudresourcemanager.googleapis.com,logging,storage_component,storage_api,bigquery,pubsub">
</walkthrough-enable-apis>

### Open the Cloud Shell

Cloud Shell is a built-in command line tool for the console. You're going to use
Cloud Shell to deploy your app.

Open Cloud Shell by clicking
<walkthrough-cloud-shell-icon></walkthrough-cloud-shell-icon>
[icon][spotlight-open-devshell] in the navigation bar at the top of the console.

## Install Cloud Dataflow samples on Cloud Shell

The Python version of Cloud Dataflow requires a development environment that has
Python, the Google Cloud SDK, and the Cloud Dataflow SDK for Python.
Additionally, Cloud Dataflow uses pip, Python's package manager, to manage SDK
dependencies.

This tutorial uses a Cloud Shell that has Python and pip already installed. If
you prefer, you can do this tutorial [on your local
machine.][dataflow-python-tutorial]

### Download the samples and the Cloud Dataflow SDK for Python using the pip command

When you run this command, pip will download and install the appropriate version
of the Cloud Dataflow SDK.

```bash
pip install --user google-cloud-dataflow
```

Run the pip command in Cloud Shell.

## Set up a Cloud Storage bucket

Cloud Dataflow uses Cloud Storage buckets to store output data and cache your
pipeline code.

### Run gsutil mb

In Cloud Shell, use the command `gsutil mb` to create a Cloud Storage bucket.

```bash
gsutil mb gs://{{project-id-no-domain}}
```

For more information about the `gsutil` tool, see the
[documentation][gsutil-docs].

## Create and launch a pipeline

In Cloud Dataflow, data processing work is represented by a *pipeline*. A
pipeline reads input data, performs transformations on that data, and then
produces output data. A pipeline's transformations might include filtering,
grouping, comparing, or joining data.

### Launch your pipeline on the Dataflow Service

Use Python to launch your pipeline on the Cloud Dataflow service. The running
pipeline is referred to as a *job.*

```bash
python -m apache_beam.examples.wordcount \
  --project {{project-id}} \
  --runner DataflowRunner \
  --temp_location gs://{{project-id-no-domain}}/temp \
  --output gs://{{project-id-no-domain}}/results/output \
  --job_name {{job-name}}
```

  *  `output` is the bucket used by the WordCount example to store the job
    results.

### Your job is running

Congratulations! Your binary is now staged to the storage bucket that you
created earlier, and Compute Engine instances are being created. Cloud Dataflow
will split up your input file such that your data can be processed by multiple
machines in parallel.

Note: When you see the "JOB_STATE_DONE" message, you can close Cloud Shell.

## Monitor your job

Check the progress of your pipeline on the Cloud Dataflow page.

### Go to the Cloud Dataflow Monitoring UI page

If you haven't already, navigate to the Cloud Dataflow Monitoring UI page.

Open the [menu][spotlight-console-menu] on the left side of the console.

Then, select the **Dataflow** section.

<walkthrough-menu-navigation sectionId="DATAFLOW_SECTION"></walkthrough-menu-navigation>

### Select your job

Click your job to view its details.

### Explore pipeline details and metrics

Explore the pipeline on the left and the job information on the right. To see
detailed job status, click [Logs][spotlight-job-logs]. Try clicking a step in
the pipeline to view its metrics.

As your job finishes, you'll see the job status change, and the Compute Engine
instances used by the job will stop automatically.

## View your output

Now that your job has run, you can explore the output files in Cloud Storage.

### Go to the Cloud Storage page

Open the [menu][spotlight-console-menu] on the left side of the console.

Then, select the **Compute Engine** section.

<walkthrough-menu-navigation sectionId=STORAGE_SECTION></walkthrough-menu-navigation>

### Go to the storage bucket

In the list of buckets, select the bucket you created earlier. If you used the
suggested name, it will be named `{{project-id}}`.

The bucket contains a staging folder and output folders. Dataflow saves the
output in shards, so your bucket will contain several output files.

## Clean up

In order to prevent being charged for Cloud Storage usage, delete the bucket you
created.

### Go back to the buckets browser

Click the [Buckets][spotlight-buckets-link] link.

### Select the bucket

Check the box next to the bucket you created.

### Delete the bucket

Click [Delete][spotlight-delete-bucket] and confirm your deletion.

## Conclusion

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

Here's what you can do next:

  *  [Read more about the Word Count example][wordcount]
  *  [Learn about the Cloud Dataflow programming model][df-pipelines]
  *  [Explore the Apache Beam SDK on GitHub][beam-sdk]

Set up your local environment:

  *  [Use Java and Eclipse to run Dataflow][df-eclipse]
  *  [Use Java and Maven to run Dataflow][df-maven]

[beam-sdk]: https://github.com/apache/beam/tree/master/sdks/python
[dataflow-python-tutorial]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-python
[df-eclipse]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-java-eclipse
[df-maven]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-java-maven
[df-pipelines]: https://cloud.google.com/dataflow/model/programming-model-beam
[gsutil-docs]: https://cloud.google.com/storage/docs/gsutil
[spotlight-buckets-link]: walkthrough://spotlight-pointer?cssSelector=.p6n-cloudstorage-path-link
[spotlight-console-menu]: walkthrough://spotlight-pointer?spotlightId=console-nav-menu
[spotlight-delete-bucket]: walkthrough://spotlight-pointer?cssSelector=#p6n-cloudstorage-delete-buckets
[spotlight-job-logs]: walkthrough://spotlight-pointer?cssSelector=#p6n-dax-job-logs-toggle
[spotlight-open-devshell]: walkthrough://spotlight-pointer?spotlightId=devshell-activate-button
[wordcount]: https://beam.apache.org/get-started/wordcount-example/
