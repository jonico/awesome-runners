# awesome-runners
A curated list of awesome self-hosted GitHub Action runner solutions in a large comparison matrix

## Purpose

The purpose of this repository is to provide an overview on self-hosted runner solutions for GitHub Actions compared by various criteria. There is no rating implied as the importance of the various categories differ from use case to use case.
Data can be out of date, so if a certain feature is told to be missing, please double check whether this is still the case.

Pull requests are welcome :blush:

### General collection of self-hosted runner best practices

During my research, I stumbled over [dduzgun-security/github-self-hosted-runners](https://github.com/dduzgun-security/github-self-hosted-runners) with :sparkles: tips on what to consider when using self-hosted runners by yourself.

## The matrix

| Solution name                                                                       | Runtime                       | GHES     | RegScope                                | Scaling                                    | AutoScaling                                             | Architecture           | Dereg                    | PATInRunner | CleanUp         | Privileged                        | Exposed                            | AllInOne                          | Contributors | SelfService                      | IdleCosts                                                                 |
|-------------------------------------------------------------------------------------|-------------------------------|----------|-----------------------------------------|--------------------------------------------|---------------------------------------------------------|------------------------|--------------------------|-------------|-----------------|-----------------------------------|------------------------------------|-----------------------------------|--------------|----------------------------------|---------------------------------------------------------------------------|
| [summerwind/actions-runner-controller](https://github.com/summerwind/actions-runner-controller)                             | k8s                           | yes      | Enterprise,Org,Repo,Labels,RunnerGroups | yes (k8s manifests and dynamic scaling)    | pending+running jobs or percentage runners already busy | x86, AMD64, ARM, ARM64 | yes                      | no          | yes (ephemeral) | yes (install time, optional DInD) | no                                 | no                                | [![GitHub contributors](https://img.shields.io/github/contributors/summerwind/actions-runner-controller.svg)](https://github.com/summerwind/actions-runner-controller/graphs/contributors/)           | yes (IssueOps project available) | actions-runner controller + at least one pod per org runner               |
| [philips-labs/terraform-aws-github-runner](https://github.com/philips-labs/terraform-aws-github-runner)                         | AWS EC2/Lambda                | yes      | Org,Repo,Labels,RunnerGroups            | yes (Terraform config and dynamic scaling) | pending jobs in org/repo                                | x86, AMD64, ARM, ARM64 | yes                      | no          | no              | no                                | yes (GitHub check_run events)      | yes (at least intended this way)  | 24           | yes (IssueOps project available) | no (only Lambdas, KMS, queue service, API gateway)                        |
| [myoung34/docker-github-actions-runner](https://github.com/myoung34/docker-github-actions-runner)                            | Docker                        | yes      | Org,Repo,Labels,RunnerGroups            | docker-compose, Nomad and k8s examples     | no                                                      | x86, ARM64, ARM        | yes                      | yes         | no              | yes (DInD)                        | no                                 | no                                | 21           | no                               | no                                                                        |
| [MonolithProjects/ansible-github_actions_runner](https://github.com/MonolithProjects/ansible-github_actions_runner)                   | bare metal/VM                 | yes      | Organization,Repo                       | based on Ansible playbook                  | no                                                      | x86, AMD64, ARM, ARM64 | explicitly in playbook   | no          | no              | install Ansible agents            | Ansible agents                     | possible                          | 8            | no                               | Ansible agents                                                            |
| [evryfs/github-actions-runner-operator](https://github.com/evryfs/github-actions-runner-operator)                            | k8s                           | possibly | Organization,Repo                       | yes (k8s manifests)                        | no                                                      | x86                    | yes                      | no          | no              | yes (install time, optional DInD) | no                                 | no                                | 6            | no                               | actions-runner controller                                                 |
| [terraform-google-modules/terraform-google-github-actions-runners](https://github.com/terraform-google-modules/terraform-google-github-actions-runners) | k8s (GKE), Docker, VMs (GCE) | no       | Repo                                    | based on Terraform config/k8s manifests    | only on k8s, based on pod CPU consumption (HPA metric)  | x86                    | only worked for Docker   | yes         | no              | no                                | no                                 | VMs could be configured like this | 5            | no                               | at least one idle runner to allow HPA to kick in based on CPU consumption |
| [SanderKnape/github-runner](https://github.com/SanderKnape/github-runner)                                        | Docker                        | no       | Org,Repo,Labels                         | k8s manifest example                       | no                                                      | x86                    | yes                      | yes         | no              | no                                | no                                 | no                                | 3            | no                               | no                                                                        |
| [cosmoconsult/github-runner-windows](https://github.com/cosmoconsult/github-runner-windows)                               | Windows Docker container      | no       | Org,Repo                                | docker compose example in blog             | no                                                      | win-x86                | replace but not remove   | yes         | no              | no                                | no                                 | no                                | 2            | no                               | no                                                                        |
| [machulav/ec2-github-runner](https://github.com/machulav/ec2-github-runner)                                       | AWS EC2                       | no       | Repo                                    | embedded in GitHub Actions workflow        | 1 runner per workflow run that requests it              | x86                    | part of Actions workflow | no          | yes (ephemeral) | no                                | embedded in GitHub Action workflow | possible                          | 2            | yes (Actions Workflow)           | no                                                                        |
| [aslafy-z/github-runner](https://github.com/aslafy-z/github-runner)                                           | (fat) Docker, AWS EC2         | no       | Repo,Labels                             | k8s and Nomad examples                     | no                                                      | x86                    | no                       | yes         | no              | optional to run DInD              | no                                 | yes (50G+ image with all tools)   | 2            | no                               | no                                                                        |
| [redhat-actions/self-hosted-runner-installer](https://github.com/redhat-actions/self-hosted-runner-installer)                      | Kubernetes (OpenShift)        | no       | Org,Repo,Labels                         | HELM chart parameters for k8s replica set  | no                                                      | x86                    | yes                      | yes         | no              | no                                | no                                 | no                                | 2            | no                               | no                                                                        |
| [peter-murray/github-actions-runner-container](https://github.com/peter-murray/github-actions-runner-container)                     | Docker                        | no       | Enterprise,Org,Repo,Labels,RunnerGroups | no                                         | no                                                      | x86                    | yes                      | yes         | yes             | no                                | no                                 | no                                | 1            | no                               | no                                                                        |
| [lts-beratung/ansible-github-action-runner](https://github.com/lts-beratung/ansible-github-action-runner)                        | bare metal or VM              | no       | Org,Repo                                | Ansible playbook                           | no                                                      | x86                    | yes                      | yes         | no              | install Ansible agents            | Ansible agents                     | possible                          | 1            | no                               | Ansible agents                                                            |
| [rakheshster/github-runner-on-ubuntu](https://github.com/rakheshster/github-runner-on-ubuntu)                              | Azure VM (ARM template)       | no       | Repo                                    | no                                         | no                                                      | x86                    | no                       | yes         | no              | no                                | no                                 | possible                          | 1            | no                               | no                                                                        |

## Comparison categories

#### Runtime - Container, Kubernetes, virtual machines

Specifies whether the self-hosted runners are running on a container, Kubernetes cluster or virtual machine. Virtual machine based runners typically have some cloud specific dependencies.

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

#### Exposed - Need for GitHub to reach parts of the runner solution via web hooks

Some centralized runner solutions rely on the ability to receive web hook events from GitHub about new jobs. This approach might not be feasible for some installations, although a reverse proxy may help.

#### AllInOne - Software installed in the self-hosted runners

GitHub's own, hosted runners have a lot of software already pre-installed. Most container based solutions follow a different philosophy where only a minimum amount of software is pre-installed.

#### Contributors - Number of contributors to the solution

While the number of contributors is not the only criteria, it is typically a good indicator for the maturity of a solution.

#### SelfService - Ability for end users to setup new runner scale sets

Some runner solutions have add-ons that allow end users to stand up new runner groups in a self-service fashion, e.g. via IssueOps.

#### IdleCosts - Costs that incur even if no jobs are running

Some solutions require certain central components to be up and running all the time or at least one idle runner to allow scaling up properly - this category provides an idea of what is needed in terms of components, not concrete $$$ costs.
