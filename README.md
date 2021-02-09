# awesome-runners
A curated list of awesome self-hosted GitHub Action runner solutions in a large comparison matrix

## Purpose

The purpose of this repository is to provide an overview on self-hosted runner solutions for GitHub Actions compared by various criteria. There is no rating implied as the importance of the various categories differ from use case to use case.
Data can be out of date, so if a certain feature is told to be missing, please double check whether this is still the case.

Pull requests are welcome :blush:

## General collection of self-hosted runner best practices

During my research, I stumbled over https://github.com/dduzgun-security/github-self-hosted-runners with :sparkles: tips on what to consider when using self-hosted runners by yourself.

## The matrix


## Comparison categories

#### Runtime - Container, Kubernetes, virtual machines

Specifies whether the self-hosted runners are running on a container, Kubernets cluster or vitrtual machine. Virtual machine based runners typically have some cloud specific dependencies.

#### GHES - GitHub Enterprise Server support

While GitHub.com is supported by all self-hosted runner solutions evaluated, not all of them support GitHub Enterprise Server yet (although supporting GitHub Enterprise Server is often just a matter on changing the API endpoint).

#### RegScope - Registration Scope

Self-hosted runners can be registered on the repo, org and enterprise level and may register with custom labels inside runner groups - but not all runner solutions provide support for all those options.

#### Scaling - Ability to specify multiple runner instances

Some self-hosted runner solutions have the ability to specify how many runners of a certain kind should be launched and whether crashed runners should be restarted.

#### AutoScaling

Some self-hosted runner solutions have the ability to scale automatically with the amount of pending jobs, busy runners, CPU utilization, ...

#### Architecture - Operating systems supported

While self-hosted action runners can support Linux (x86, ARM, ARM64), Mac and Windows - most self-hosted runner solutions are restricted to a subset of those architectures

#### Dereg - Automatic Runner Deregistration

Not all runner solutions remove themselves after they have been deleted, which can be problematic, especially, if combined aith auto-scaling capabilities.

#### SecretInRunner - Personal access or OAuth token needed in runner

Some runner solutions provide a personal access token (PAT) or OAuth token directly to the runner so that it can register itself. This imposes the risk of a malicious job trying to steal the token and use it to elevate its permissions. Solutions that only pass a runner token to the actual runners are preferred from a security perspective.

#### CleanUp - Automated clean up after a build

While self-hosted runner provide some isolation between jobs, it is the responsibility of the job to clean up in most cases. Some self-runner solutions automatically de-register and clean-up runners after every build to avoid any interference between jobs.


#### Privileged - Any special privileges needed to run or install the solution

Calls out any special privileges (like Kubernetes cluster admin, Docker privileged mode) needed to run or install the solution.

#### Docker - Running and building docker images with docker supported

Many GitHub Actions rely on Docker being present on the runner. As running Docker in Docker can be a significant security risk, there should also be an option to disable Docker in container based solutions.

#### Exposed - Need for GitHub to reach parts of the runner solution via web hooks

Some centralized runner solutions rely on the ability to receive web hook events from GitHub about new jobs. This approach might not be feasible for some installations, although a reverse proxy may help.

#### AllInOne - Software installed in the self-hosted runners

GitHub's own, hosted runners have a lot of software already pre-installed. Most container based solutions follow a different philosophy where only a minimum amount of software is pre-installed.

#### Community - Number of contributors to the solution

While the number of contributors is not the only criteria, it is typically a good indicator for the maturity of a solution.

#### SelfService - Ability for end users to setup new runner scale sets

Some runner solutions have add-ons that allow end users to stand up new runner groups in a self-service fashion, e.g. via IssueOps.

#### IdleCosts - Costs that incur even if no jobs are running

Some solutions require certain central components to be up and running all the time or at least one idle runner to allow scaling up properly - this category provides an idea of what is needed in terms of components, not concrete $$$ costs.
