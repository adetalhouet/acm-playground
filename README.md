_Disclaimer_: the content in this repository is **not** Red Hat supported. This is provided for demo purposes only.

## Advanced Cluster Management playground

#### Pre-requisite

Create a namespace to host the acm objects (Channel)

	oc create ns acm-playground

Apply the channel to leverage this repository as a Git source		

	oc apply -f channel.yaml

#### App

##### Pacman

###### Prerequisites
1. It is required to have Ansbile Tower installed. See [here](https://releases.ansible.com/ansible-tower/setup_openshift/) on how to install Tower on OpenShift
2. Update `apps/00-tower-setup/tower_cli-EXAMPLE.cfg` according to your environment and rename it `apps/00-tower-setup/tower_cli.cfg`
3. Create a [ServiceNow developer account](https://developer.servicenow.com/dev.do) and update `apps/00-tower-setup/group_var/credentials.yaml` accordingly
4. Run `ansible-playbook apps/00-tower-setup/tower-setup.yml` to create the various elements required in Tower.

###### Install the app
Create your tower secret file `apps/pacman/tower-secret.yaml` based on the example, and seal the secret. If you don't have a way to seal the secret, then apply it manually, or [deploy sealed secret](https://github.com/adetalhouet/ocp-gitops/tree/main/apps/02-sealed-secrets)

	kubeseal --cert ~/.bitnami/tls.crt --format yaml < apps/pacman/tower-secret.yaml > apps/pacman/sealed-tower-secret.yaml

Apply the subscription to register for the `pacman-app`

	oc apply -f apps/pacman-app.yaml

#### Policies

Create a namespace to host the policies

	oc create ns policies

Apply the subscription to register for the `policies`

	oc apply -f policies-subscription.yaml

See all deployed policies

	oc get Policy,PlacementRule,PlacementBinding -n policies

Delete all policies manually

	oc get Policy,PlacementRule,PlacementBinding -n policies -o name | xargs oc delete -n policies
