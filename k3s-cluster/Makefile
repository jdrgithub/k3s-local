# k3s-cluster/Makefile

# USAGE
# make install-k3s ENV=dev       <- Install dev instance of k3s
# make status                    <- See nodes & pods
# make uninstall-k3s             <- Tear it down
# make reinstall-k3s             <- Reset
# make create-namespaces         <- Apply namespaces.yaml
# make add-helm-repos            <- Add Helm repos

ENV ?= dev

INSTALL_FLAGS := --disable=traefik
UNINSTALL_SCRIPT := /usr/local/bin/k3s-uninstall.sh

# PATHS FOR BOOTSTRAP
BOOTSTRAP_DIR := envs/cluster-bootstrap
NAMESPACES := $(BOOTSTRAP_DIR)/namespaces.yaml

install-k3s:
	@echo "+ Installing k3s ($(ENV))..."
	curl -sfL https://get.k3s.io | INSTALL_K3S_EXEC="$(INSTALL_FLAGS)" sh -
	@echo "k3s installed"

uninstall-k3s:
	@echo "+ Uninstalling k3s..."
	-[ -f $(UNINSTALL_SCRIPT) ] && sudo $(UNINSTALL_SCRIPT)
	@echo "k3s uninstalled"

reinstall-k3s: uninstall-k3s install-k3s

create-namespaces:
	@echo "+ Applying namespaces from $(NAMESPACES)..."
	kubectl apply -f $(NAMESPACES)

add-helm-repos:
	@echo "+ Adding Helm repos (hardcoded for now)..."
	helm repo update

status:
	kubectl get nodes
	kubectl get pods -A

