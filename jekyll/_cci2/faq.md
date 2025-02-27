---
layout: classic-docs
title: "FAQ"
short-title: "FAQ"
description: "Frequently asked questions about CircleCI"
categories: [migration]
order: 1
version:
- Cloud
- Server v3.x
- Server v2.x
---

* TOC
{:toc}

## General
{: #general }

### Does CircleCI look at my code?
{: #does-circleci-look-at-my-code }
{:.no_toc}
CircleCI employees never look at your code without permission. If you have requested support, a support engineer may ask permission to look at your code to help you debug the problem.

See the CircleCI [security policy]({{ site.baseurl }}/2.0/security/) for more details.

## Migration
{: #migration }

Visit the [Migration Introduction]({{ site.baseurl }}/2.0/migration-intro/) doc which links out to migration guides for various platforms.

### How do I migrate from Jenkins to CircleCI?
{: #how-do-i-migrate-from-jenkins-to-circleci }
{:.no_toc}

Refer to [Migrating From Jenkins]({{ site.baseurl }}/2.0/migrating-from-jenkins/) for conceptual differences between Jenkins and CircleCI.

### Can I use CircleCI without creating base images?
{: #can-i-use-circleci-without-creating-base-images }
{:.no_toc}

Yes, CircleCI provides a selection of "convenience images" for use with the Docker executor. For a full list, along with usage instructions, visit the [CircleCI Developer Hub](https://circleci.com/developer/images) and the [CircleCI Images guide]({{site.baseurl}}/2.0/circleci-images/).

For the `machine` executor, see the [available machine images]({{site.baseurl}}/2.0/configuration-reference/#available-linux-machine-images) list.

For an introduction to execution environments and images, see the [Executors and Images guide]({{site.baseurl}}/2.0/executor-intro/).

## Hosting
{: #hosting }

### Is CircleCI available to enterprise customers?
{: #is-circleci-20-available-to-enterprise-customers }
{:.no_toc}
Yes, CircleCI server is available for installation on AWS or GCP. See the [CircleCI Server v3.x Overview]({{ site.baseurl }}/2.0/server-3-overview) for details and links to installation instructions. [Contact us](https://circleci.com/pricing/server/) to discuss your requirements.

### What are the differences between CircleCI’s hosting options?
{: #what-are-the-differences-between-circlecis-hosting-options }
{:.no_toc}
- **Cloud** - CircleCI manages the setup, infrastructure, security and maintenance of your services. You get instant access to new feature releases and automatic upgrades, alleviating the need for manual work on an internal system.

- **Server** - You install and manage CircleCI, through a service like AWS or GCP. Server installations are behind a firewall that your team sets up and maintains according to your datacenter policy. You have full administrative control for complete customization and management of upgrades as new versions are released.

## Troubleshooting
{: #troubleshooting }

### Why aren't my jobs running when I push commits?
{: #why-arent-my-jobs-running-when-i-push-commits }
{:.no_toc}
In the CircleCI application, check the individual job and workflow views for error messages. More often than not, the error is because of formatting errors in your `config.yml` file.

See [Writing YAML]({{ site.baseurl }}/2.0/writing-yaml/) for more details.

After checking your `config.yml` for formatting errors, search for your issue in the [CircleCI support center](https://support.circleci.com/hc/en-us).

### Why is my job queued?
{: #why-is-my-job-queued }
{:.no_toc}
A job might end up being **queued** because of a concurrency limit being imposed due to the plan you or your organisation are on. If your jobs are queuing often, you can consider [upgrading your plan](https://circleci.com/pricing/).


### Why are my builds queuing even though I'm on the Performance plan?
{: #why-are-my-builds-queuing-even-though-im-on-performance-plan }
{:.no_toc}
In order to keep the system stable for all CircleCI customers, we implement different soft concurrency limits on each of the [resource classes]({{site.baseurl}}/2.0/configuration-reference/#resource_class). If you are experiencing queuing on your jobs, it is possible you are hitting these limits. Please [contact CircleCI support](https://support.circleci.com/hc/en-us/requests/new) to request raises on these limits.

### Why can't I find my project on the Projects dashboard?
{: #why-cant-i-find-my-project-on-the-projects-dashboard }
{:.no_toc}
If you are not seeing a project you would like to build, and it is not currently building on CircleCI, check your org in the top left corner of the CircleCI application.  For instance, if the top left shows your user `my-user`, only GitHub projects belonging to `my-user` will be available under `Projects`.  If you want to build the GitHub project `your-org/project`, you must change your org on the application Switch Organization menu to `your-org`.

### I got an error saying my “build didn’t run because it needs more containers than your plan allows” but my plan has more than enough. Why is this failing?
{: #i-got-an-error-saying-my-build-didnt-run-because-it-needs-more-containers-than-your-plan-allows-but-my-plan-has-more-than-enough-why-is-this-failing }
{:.no_toc}
There is a default setting within CircleCI to initially limit project parallelism to 16. If you request more than that, it will fail. Contact [Support or your Customer Success Manager](https://support.circleci.com/hc/en-us) to have it increased.

### How do Docker image names work? Where do they come from?
{: #how-do-docker-image-names-work-where-do-they-come-from }
{:.no_toc}
CircleCI currently supports pulling (and pushing with Docker Engine) Docker images from [Docker Hub][docker-hub]. For [official images][docker-library], you can pull by simply specifying the name of the image and a tag:

```
golang:1.7.1-jessie
redis:3.0.7-jessie
```

For public images on Docker Hub, you can pull the image by prefixing the account or team username:

```
my-user/couchdb:1.6.1
```

### What is the best practice for specifying image versions?
{: #what-is-the-best-practice-for-specifying-image-versions }
{:.no_toc}
It is best practice **not** to use the `latest` tag for specifying image versions. It is also best practice to use a specific version and tag, for example `circleci/ruby:2.4-jessie-node`, to pin down the image and prevent upstream changes to your containers when the underlying base distro changes. Specifying only `circleci/ruby:2.4` could result in unexpected changes from `jessie` to `stretch` for example. For more context, refer to the [Docker Image Best Practices]({{ site.baseurl }}/2.0/using-docker/#docker-image-best-practices) section of the Choosing an Executor Type document and the Best Practices section of the [CircleCI Images]({{ site.baseurl }}/2.0/circleci-images/#best-practices) document.

### How can I set the timezone in Docker images?
{: #how-can-i-set-the-timezone-in-docker-images }
{:.no_toc}
You can set the timezone in Docker images with the `TZ` environment variable. In your `.circleci/config.yml`, it would look like:

A sample `.circleci/config.yml` with a defined `TZ` variable would look like this:

```yaml
version: 2
jobs:
  build:
    docker:
      - image: your/primary-image:version-tag
        auth:
          username: mydockerhub-user
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
      - image: mysql:5.7
        auth:
          username: mydockerhub-user
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
        environment:
           TZ: "America/Los_Angeles"
    working_directory: ~/your-dir
    environment:
      TZ: "America/Los_Angeles"
```

In this example, the timezone is set for both the primary image and an additional mySQL image.

A full list of available timezone options is [available on Wikipedia](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

## Workflows
{: #workflows }

### Can I use the API with Workflows?
{: #can-i-use-the-api-with-workflows }
{:.no_toc}
Yes. Refer to the [Enabling Pipelines]({{ site.baseurl }}/2.0/build-processing/) document for instructions and links to the API endpoint.

### Can I use the Auto-cancel feature with Workflows?
{: #can-i-use-the-auto-cancel-feature-with-workflows }
{:.no_toc}
Yes, see the [Skipping and Cancelling Builds]({{ site.baseurl }}/2.0/skip-build/) document for instructions.

### Can I use `store_test_results` with Workflows?
{: #can-i-use-storetestresults-with-workflows }
{:.no_toc}
You can use `store_test_results` in order to populate your Test Summary section with test results information and for [timing-based test-splitting]({{ site.baseurl }}/2.0/parallelism-faster-jobs/#splitting-by-timing-data). Test timings data is available for 2.0 with Workflows, using data from a job with the same name going back 50 builds.

### Can I use Workflows with the Installable CircleCI?
{: #can-i-use-workflows-with-the-installable-circleci }
{:.no_toc}
Yes, Workflows are available in CircleCI as part of the 2.0 option for enterprise clients. Refer to the [Administrator's Overview]({{ site.baseurl }}/2.0/overview) for installation instructions.

### How many jobs can I run at one time?
{: #how-many-jobs-can-i-run-at-one-time }
{:.no_toc}
The number of containers in your plan determines the number of jobs that may be run at one time. For example, if you have ten workflow jobs ready to run, but only five containers in your plan, only five jobs will run.
Using Workflow config you can run multiple jobs at once or sequentially. You can fan-out (run multiple jobs at once) or fan-in (wait for all the jobs to complete before executing the dependent job).

### Do you plan to add the ability to launch jobs on both Linux and Mac environments in the same workflow?
{: #do-you-plan-to-add-the-ability-to-launch-jobs-on-both-linux-and-mac-environments-in-the-same-workflow }
{:.no_toc}
Yes, this is supported. See the section for multiple executor types in the [Sample 2.0 `config.yml` Files]({{ site.baseurl }}/2.0/sample-config/#sample-configuration-with-multiple-executor-types) document.

### Is it possible to split the `config.yml` into different files?
{: #is-it-possible-to-split-the-configyml-into-different-files }
{:.no_toc}
Splitting `config.yml` into multiple files is not yet supported.

### Can I build only the jobs that changed?
{: #can-i-build-only-the-jobs-that-changed }
{:.no_toc}
No.

### Can I build fork PR’s using Workflows?
{: #can-i-build-fork-prs-using-workflows }
{:.no_toc}
Yes!

### Can workflows be scheduled to run at a specific time of day?
{: #can-workflows-be-scheduled-to-run-at-a-specific-time-of-day }
{:.no_toc}
Yes, for the CircleCI hosted application. For example, to run a workflow at 4 PM use `"0 16 * * *"` as the value for the `cron:` key. Times are interpreted in the UTC time zone.

### What time zone is used for schedules?
{: #what-time-zone-is-used-for-schedules }
{:.no_toc}
Coordinated Universal Time (UTC) is the time zone in which schedules are interpreted.

### Why didn’t my scheduled build run?
{: #why-didnt-my-scheduled-build-run }
{:.no_toc}
You must specify exactly the branches on which the scheduled workflow will run and push that 'config.yml' to the branch you want to build. A push on the `master` branch will only schedule a workflow for the `master` branch.

### Can I schedule multiple workflows?
{: #can-i-schedule-multiple-workflows }
{:.no_toc}
Yes, every workflow with a `schedule` listed in the `trigger:` key will be run on the configured schedule.

### Are scheduled workflows guaranteed to run at precisely the time scheduled?
{: #are-scheduled-workflows-guaranteed-to-run-at-precisely-the-time-scheduled }
{:.no_toc}
CircleCI provides no guarantees about precision. A scheduled workflow will be run as though a commit was pushed at the configured time.

## Windows
{: #windows }

### What do I need to get started building on Windows?
{: #what-do-i-need-to-get-started-building-on-windows }
{:.no_toc}
See the [comparison table](https://circleci.com/pricing/#comparison-table) for which plans can access Windows resources.

### What exact version of Windows are you using?
{: #what-exact-version-of-windows-are-you-using }
{:.no_toc}

The [Hello World on Windows]({{site.baseurl}}/2.0/hello-world-windows/) page outlines which versions of Windows are offered.

### What is installed on the machine?
{: #what-is-installed-on-the-machine }
{:.no_toc}

The [full list of available dependencies]({{site.baseurl}}/2.0/hello-world-windows/#software-pre-installed-in-the-windows-image) can be found in our [Hello World On Windows]({{site.baseurl}}/2.0/hello-world-windows/) page.

### Is Windows available on CircleCI server?
{: #is-windows-available-on-installed-versions-of-circleci }
{:.no_toc}

The Windows executor is available on CircleCI server v3.x and v2.x

## Billing
{: #billing }

### Credit Usage Plans
{: #credit-usage-plans }
{:.no_toc}
Visit our [Pricing page](https://circleci.com/pricing/) to learn more about the details of our plans.

#### What are credits?
{: #what-are-credits }
{:.no_toc}
Credits are used to pay for users and usage based on machine type, size, and features such as Docker Layer Caching.

For example, the 25,000 credit package would provide 2,500 build minutes when using a Docker or Linux "medium" compute at 10 credits per minute. CircleCI provides multiple compute sizes so you can optimize builds between performance (improved developer productivity) and value.

When applicable, build time can be further reduced by using parallelism, which splits the job into multiple tests that are executed at the same time. With 2x parallelism, a build that usually runs for 2,500 minutes could be executed in 1,250 minutes, further improving developer productivity. Note that when two executors are running in parallel for 1,250 minutes each, total build time remains 2,500 minutes.

#### Is there a way to share plans across organizations and have them billed centrally?
{: #is-there-a-way-to-share-plans-across-organizations-and-have-them-billed-centrally }
{:.no_toc}
Yes, log in to the CircleCI web app > select `Plan` in the sidebar > click `Share & Transfer`.

On non-free plans, you can share your plan with free organizations for which you have admin access using the `Add Shared Organization` option. All orgs you have shared your plan with will then be listed on the Share & Transfer page and child organizations will bill all credits and other usage to the parent org.

On non-free plans, you can transfer your plan to another free organization for which you have admin access using the `Transfer Plan` option. When you transfer a paid plan to another org, your org will be downgraded to the free plan.

#### If a container is used for under one minute, do I have to pay for a full minute?
{: #if-a-container-is-used-for-under-one-minute-do-i-have-to-pay-for-a-full-minute }
{:.no_toc}
You pay to the next nearest credit. First we round up to the nearest second, and then up to the nearest credit.

#### How do I buy credits? Can I buy in any increments?
{: #how-do-i-buy-credits-can-i-buy-in-any-increments }
{:.no_toc}
Every month, you are charged for your selected credit package at the beginning of the month.

#### What do I pay for?
{: #what-do-i-pay-for }
{:.no_toc}
You can choose to pay for premium features per active user, compute, and optionally, premium support.


- Access to features, such as new machine sizes, are paid with a monthly fee of 25,000 credits per active user (not including applicable taxes).
- Compute is paid for monthly in credits for the machine size and duration you use:
  - Credits are sold in packages of 25,000 at $15 each (not including applicable taxes).
  - Credits rollover each month and expire after one year.
- Docker Layer Caching (DLC) is paid for with credits per usage, similar to
  compute credits.

#### How do I calculate my monthly storage and network costs?
{: #how-do-I-calculate-my-monthly-storage-and-network-costs }
{:.no_toc}

**NOTE:** For our monthly Performance plan customers: billing for network egress and storage will start to take effect on **May 1, 2022**, based on your billing date (subject to change). CircleCI is adding variables and controls to help you manage network and storage usage, which will be available to use and test **April 1, 2022**. The information in this section is applicable after the changes take effect on May 1, 2022. Current usage can be found on the [CircleCI web app](https://app.circleci.com/) by navigating to **Plan > Plan Usage**.
{: class="alert alert-info" }

Calculate your monthly storage and network costs by finding your storage and network usage on the [CircleCI web app](https://app.circleci.com/) by navigating to **Plan > Plan** Usage.

##### Storage
{: #storage }
{:.no_toc}

To calculate monthly storage costs from your daily usage, click on the **Storage** tab to see if your organization has accrued any overages beyond the GB-monthly allotment. Your overage (GB-Months/TB-Months) can be multiplied by 420 credits to estimate the total monthly costs. Example: 2 GB-Months overage x 420 credits = 840 credits ($.50).

##### Network
{: #network }
{:.no_toc}

Billing for network usage is only applicable to traffic from CircleCI to self-hosted runners. [Read More]({{site.baseurl}}/2.0/persist-data/#overview-of-storage-and-network-transfer).

Your network overage GB/TB can be multiplied by 420 credits to estimate the total monthly costs. Example: 2 GB-Months overage x 420 credits = 840 credits ($.50).

#### How do I calculate my monthly IP ranges cost?
{: #how-do-I-calculate-my-monthly-IP-ranges-costs }
{:.no_toc}

Calculate your monthly IP ranges costs by finding your IP ranges usage on the [CircleCI app](https://app.circleci.com/) by navigating to Plan > Plan Usage.

In addition to the **IP Ranges Usage** summary, you can navigate to the **IP Ranges** tab to find more details about your data usage. In this tab, the IP ranges usage value represents the raw number of bytes in or out of the Docker container during execution of a job with IP ranges enabled.

This number includes the job's overall network transfer _and_ any other bytes that go in or out of the Docker container. Data used to pull in the Docker image to the container before the job starts executing will _not incur usage costs_ for jobs with IP ranges enabled.

This feature will consume 450 credits from your account for each GB of data used for jobs with IP ranges enabled. You can also view job-specific details of IP ranges usage in the **Resources** tab on the **Job Details** UI page. See [IP ranges pricing]({{site.baseurl}}/2.0/ip-ranges/#pricing) for more information.

#### How do I predict my monthly IP ranges cost without enabling the feature first?

You can view an approximation of network transfer for any Docker job (excluding Remote Docker) in the Resources tab on the Job Details UI page.  Convert this value to GB if it is not already in GB and multiply by 450 credits to predict the approximate cost of enabling IP ranges on that Docker job.

#### Why does CircleCI have per-active-user pricing?
{: #why-does-circleci-have-per-active-user-pricing }
{:.no_toc}

Credit usage covers access to compute. We prefer to keep usage costs as low as possible to encourage frequent job runs, which is the foundation of a good CI practice. Per-active-user fees cover access to platform features and job orchestration. This includes features like dependency caching, artifact caching, and workspaces, all of which speed up build times without incurring additional compute cost.

#### What constitutes an _Active User_?
{: #what-constitutes-an-active-user }
{:.no_toc}

An `active user` is any user who triggers the use of compute resources on non-OSS projects. This includes activities such as:

- Commits from users that trigger builds, including PR Merge commits.
- Re-running jobs in the CircleCI web application, including [SSH debug]({{ site.baseurl }}/2.0/ssh-access-jobs).
- Approving [manual jobs]({{ site.baseurl }}/2.0/workflows/#holding-a-workflow-for-a-manual-approval) (approver will be considered the actor of all downstream jobs).
- Using scheduled workflows
- Machine users

**Note:** If your project is [open-source]({{ site.baseurl }}/2.0/oss) you will **not** be considered an active user.

To find a list of your Active Users, log in to the CircleCI web app > click `Plan` > click `Plan Usage` > click on the `Users` tab.

#### What happens when I run out of credits?
{: #what-happens-when-i-run-out-of-credits }
{:.no_toc}

On the **Performance plan**, when you reach 2% of your remaining credits, you will be refilled 25% of your credit subscription, with a minimum refill of 25,000 credits. For example, If your monthly package size is 100,000 credits, you will automatically be refilled 25,000 credits (at $.0006 each, not including applicable taxes) when you reach 2000 remaining credits.

If you notice that your account is receiving repeated refills, review your credit usage by logging in to the CircleCI web app > click `Plan` > click `Plan Usage`. In most cases, increasing your credit package should minimize repeat refills. You can manage your plan by clicking `Plan Overview`.

On the **Free plan**, jobs will fail to run once you have run out of credits.

#### Do credits expire?
{: #do-credits-expire }
{:.no_toc}
**Performance plan**: Credits expire one year after purchase. Unused credits will be forfeited when the account subscription is canceled.

#### How do I pay?
{: #how-do-i-pay }
{:.no_toc}
You can pay from inside the CircleCI app for monthly pricing.

#### When do I pay?
{: #when-do-i-pay }
{:.no_toc}

On the **Performance plan**, at the beginning of your billing cycle, you will be charged for premium support tiers and your monthly credit allocation. Any subsequent credit refills _during_ the month (such as the auto-refilling at 25% on reaching 2% of credits available) will be paid _at the time of the refill_.

#### Am I charged if my build is "Queued" or "Preparing"?
{: #am-i-charged-if-my-build-is-queued-or-preparing }

No. If you are notified that a job is "queued", it indicates that your job is
waiting due to a **plan** or **concurrency** limit. If your job indicates that
it is "preparing", it means that CircleCI is setting up or _dispatching_ your
job so that it may run.

#### What are the other renewal dates?
{: #what-are-the-other-renewal-dates }
{:.no_toc}

The first credit card charge on the day you upgrade to a paid plan or change paid plans, in addition to the following charges from CircleCI:

- On the monthly renewal date if your team is on the monthly plan.
- On the annual renewal date if your team is on the annual plan.
- On the last day of the month if your team is on the annual plan and there is an outstanding balance from adding new users or utilizing more credits.
- If you are on the Performance plan, anytime your team’s credit balance drops below your preset limit, another credit purchase will be processed.

#### Are there credit plans for open source projects?
{: #are-there-credit-plans-for-open-source-projects }
{:.no_toc}

Open source organizations **on our Free plan** receive 400,000 free credits per month that can be spent on Linux open source projects.  Open-source credit availability and limits will not be visible in the UI.

If you build on macOS, we also offer organizations on our Free plan 25,000 free credits per month to use on macOS open source builds. For access to this, contact our team at billing@circleci.com. Free credits for macOS open source builds can be used on a maximum of 2 concurrent jobs per organization.

#### I currently get free credits for open source projects on my container plan. How do I get discounts for open source on the Performance plan?
{: #i-currently-get-free-credits-for-open-source-projects-on-my-container-plan-how-do-i-get-discounts-for-open-source-on-the-performance-plan }
{:.no_toc}

CircleCI no longer offers discounts for open source customers on the Performance plan.

#### Why does CircleCI charge for Docker layer caching?
{: #why-does-circleci-charge-for-docker-layer-caching }
{:.no_toc}

Docker layer caching (DLC) reduces build times on pipelines where Docker images are
built by only rebuilding Docker layers that have changed (read more about DLC
[here]({{site.baseurl}}/2.0/docker-layer-caching). DLC costs 200 credits per
job run.

There are a few things that CircleCI does to ensure DLC is available to
customers. We use solid-state drives and replicate the cache across zones to
make sure DLC is available. We will also increase the cache as needed in order
to manage concurrent requests and make DLC available for your jobs. All of these
optimizations incur additional cost for CircleCI with our compute providers,
which pass along to customers when they use DLC.

To estimate your DLC cost, look at the jobs in your config file with Docker
layer caching enabled, and the number of Docker images you are building in those
jobs. There are cases where a job can be written once in a config file but the
job runs multiple times in a pipeline, for example, with parallelism enabled.

Note that the benefits of Docker layer caching are only apparent on pipelines
that are building Docker images, and reduces image build times by reusing the
unchanged layers of the application image built during your job. If your
pipeline does not include a job where Docker images are built, Docker layer
caching will provide no benefit.

---

### Container Based Plans
{: #container-based-plans }

#### How do I upgrade my container plan with more containers to prevent queuing?
{: #how-do-i-upgrade-my-container-plan-with-more-containers-to-prevent-queuing }
{:.no_toc}
* Linux: Go to the Settings > Plan Settings page of the CircleCI app to increase the number of containers on your Linux plan. Type the increased number of containers in the entry field under the Choose Linux Plan heading and click the Pay Now button to enter your payment details.

#### Is there a way to share plans across organizations and have them billed centrally?
{: #is-there-a-way-to-share-plans-across-organizations-and-have-them-billed-centrally }
{:.no_toc}
Yes, go to the Settings > Share & Transfer > Share Plan page of the CircleCI app to select the Orgs you want to add to your plan.

#### Can I set up billing for an organization, without binding it to my personal account?
{: #can-i-set-up-billing-for-an-organization-without-binding-it-to-my-personal-account }
{:.no_toc}
Yes, the billing is associated with the organization. You can buy while within that org's context from that org's settings page. But, you must have another GitHub Org Admin who will take over if you unfollow all projects. We are working on a better solution for this in a future update.

#### What is the definition of a container in the context of billing?
{: #what-is-the-definition-of-a-container-in-the-context-of-billing }
{:.no_toc}
A container is a 2 CPU 4GB RAM machine that you pay for access to. Containers may be used for concurrent tasks (for example, running five different jobs) or for parallelism (for example, splitting one job across five different tasks, all running at the same time). Both examples would use five containers.

#### Why am I being charged for remote Docker spin up time?
{: #why-am-i-being-charged-for-remote-docker-spin-up-time }
{:.no_toc}
When CircleCI spins up a remote docker instance, it requires the primary container to be running and spending compute. Thus while you are not charged for the remote docker instance itself, you are charged for the time that the primary container is up.

---

## Architecture
{: #architecture }

### Can I use IPv6 in my tests?
{: #can-i-use-ipv6-in-my-tests }
{:.no_toc}
You can use the [machine executor]({{ site.baseurl }}/2.0/configuration-reference/#machine) for testing local IPv6 traffic.  Unfortunately, we do not support IPv6 internet traffic, as not all of our cloud providers offer IPv6 support.

Hosts running with machine executor are configured with IPv6 addresses for `eth0` and `lo` network interfaces.

You can also configure Docker to assign IPv6 address to containers, to test services with IPv6 setup.  You can enable it globally by configuring docker daemon like the following:

```yaml
jobs:
  ipv6_tests:
  machine:
    # The image uses the current tag, which always points to the most recent
    # supported release. If stability and determinism are crucial for your CI
    # pipeline, use a release date tag with your image, e.g. ubuntu-2004:202201-02
    image: ubuntu-2004:current
  steps:
    - checkout
    - run:
        name: enable ipv6
        command: |
          cat <<'EOF' | sudo tee /etc/docker/daemon.json
          {
            "ipv6": true,
            "fixed-cidr-v6": "2001:db8:1::/64"
          }
          EOF
          sudo service docker restart
```

Docker allows enabling IPv6 at different levels: [globally via daemon config like above](https://docs.docker.com/engine/userguide/networking/default_network/ipv6/), with [`docker network create` command](https://docs.docker.com/engine/reference/commandline/network_create/), and with [`docker-compose`](https://docs.docker.com/compose/compose-file/#enable_ipv6).


### What operating systems does CircleCI support?
{: #what-operating-systems-does-circleci-20-support }
{:.no_toc}
- **Linux:** CircleCI is flexible enough that you should be able to build most applications that run on Linux. These do not have to be web applications!

- **Android:** Refer to [Android Language Guide]({{ site.baseurl }}/2.0/language-android/) for instructions.

- **iOS:** Refer to the [iOS Project Tutorial]({{ site.baseurl }}/2.0/ios-tutorial) to get started.

- **Windows:** We are currently offering Early Access to Windows. Please take a look at [this Discuss post](https://discuss.circleci.com/t/windows-early-access-now-available-on-circleci/30977) for details on how to get access.

### Which CPU architectures does CircleCI support?
{: #which-cpu-architectures-does-circleci-support }
{:.no_toc}
CircleCI supports `amd64` for Docker jobs, and both `amd64` and [ARM resources]({{ site.baseurl }}/2.0/arm-resources/) for machine jobs.


[docker-hub]: https://hub.docker.com
[docker-library]: https://hub.docker.com/explore/
