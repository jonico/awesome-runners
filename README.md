# awesome-runners
A curated list of awesome self-hosted GitHub Action runner solutions in a large comparison matrix

## Purpose

The purpose of this repository is to provide an overview on self-hosted runner solutions for GitHub Actions compared by various criteria. There is no rating implied as the importance of the various categories differ from use case to use case.
Data can be out of date, so if a certain feature is told to be missing, please double check whether this is still the case.

Pull requests are welcome :blush:

### General collection of self-hosted runner best practices

During my research, I stumbled over https://github.com/dduzgun-security/github-self-hosted-runners with :sparkles: tips on what to consider when using self-hosted runners by yourself.

## The matrix

| Solution name                                                                       | Runtime                                 | GHES | RegScope                                | Scaling                                        | AutoScaling                                      | Architecture    | Dereg                  | PATInRunner | CleanUp | Privileged                      | Docker       | Exposed                       | AllInOne                         | Community       | SelfService                      | IdleCosts                                                                 |
|-------------------------------------------------------------------------------------|-----------------------------------------|------|-----------------------------------------|------------------------------------------------|--------------------------------------------------|-----------------|------------------------|-------------|---------|---------------------------------|--------------|-------------------------------|----------------------------------|-----------------|----------------------------------|---------------------------------------------------------------------------|
| https://github.com/philips-labs/terraform-aws-github-runner                         | AWS EC2/Lambda                          | yes  | Org,Repo,Labels,RunnerGroups            | yes (Terraform config and dynamic scaling)     | pending jobs                                     | x86, ARM, ARM64 | yes                    | no          | no      | no                              | yes          | yes (GitHub check_run events) | yes (at least intended this way) | 24 contributors | yes (IssueOps project available) | no (only Lambdas)                                                         |
| https://github.com/summerwind/actions-runner-controller                             | Kubernetes                              | yes  | Enterprise,Org,Repo,Labels,RunnerGroups | yes (Kubernetes manifests and dynamic scaling) | pending jobs/runners already busy                | x86, ARM, ARM64 | yes                    | no          | yes     | yes (install time and for DInD) | configurable | no                            | no                               | 38 contributors | yes (IssueOps project available) | actions-runner controller + at least one pod per org runner               |
| https://github.com/terraform-google-modules/terraform-google-github-actions-runners | Kubernetes (GKE), Docker and VMs (GCE)  | no   | Repo                                    | based on Terraform config/Kubernetes manifests | only on Kubernetes, based on pod CPU consumption | x86             | only worked Kubernetes | yes         | no      | no                              | configurable | no                            | no                               | 5 contributors  | no                               | at least one idle runner to allow HPA to kick in based on CPU consumption |
| https://github.com/evryfs/github-actions-runner-operator                            | Kubernetes                              | no   |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/MonolithProjects/ansible-github_actions_runner                   | bare metal/VM                           | yes  |                                         |                                                |                                                  |                 |                        | no          |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/machulav/ec2-github-runner                                       | AWS EC2                                 |      |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/aslafy-z/github-runner                                           |                                         |      |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/XenitAB/github-runner                                            |                                         |      |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/redhat-actions/self-hosted-runner-installer                      |                                         |      |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/lts-beratung/ansible-github-action-runner                        |                                         |      |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/rakheshster/github-runner-on-ubuntu                              |                                         |      |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |
| https://github.com/SanderKnape/github-runner                                        |                                         |      |                                         |                                                |                                                  |                 |                        |             |         |                                 |              |                               |                                  |                 |                                  |                                                                           |

## Comparison categories

#### Runtime - Container, Kubernetes, virtual machines

Specifies whether the self-hosted runners are running on a container, Kubernets cluster or virtual machine. Virtual machine based runners typically have some cloud specific dependencies.

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

#### PATInRunner - Personal access or OAuth token needed in runner

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
