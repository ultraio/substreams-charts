POLICY_CONFIG ?= ../../.github/datree/policies.yaml

dependency:
	#Noop target

lint: 
	helm lint

test:
	helm template . | datree test - --policy-config ${POLICY_CONFIG}

.PHONY: dependency lint test