## Advanced Cluster Management playground

#### Pre-requisite

Create a namespace to host the acm objects (Channel)

	oc create ns acm-playground

Apply the channel to leverage this repository as a Git source		

	oc apply -f channel.yaml

#### Policies

Create a namespace to host the policies

	oc create ns policies

Apply the subscription to register for the `policies`

	oc apply -f policies-subscription.yaml

See all deployed policies

	oc get Policy,PlacementRule,PlacementBinding -n policies

Delete all policies manually

	oc get Policy,PlacementRule,PlacementBinding -n policies -o name | xargs oc delete -n policies