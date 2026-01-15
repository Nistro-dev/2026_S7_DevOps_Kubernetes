.PHONY: deploy url clean test-volume status pods deployments services pvc hpa describe logs

deploy:
	kubectl apply -k .

url:
	minikube service wordpress --url

clean:
	kubectl delete -k . 2>/dev/null || true
	kubectl delete pvc mysql-storage-mysql-0 wordpress-storage 2>/dev/null || true

status:
	@echo "=== Deployments ==="
	@kubectl get deployments
	@echo "\n=== Pods ==="
	@kubectl get pods
	@echo "\n=== Services ==="
	@kubectl get services
	@echo "\n=== PVC ==="
	@kubectl get pvc
	@echo "\n=== HPA ==="
	@kubectl get hpa

pods:
	kubectl get pods -o wide

deployments:
	kubectl get deployments -o wide

services:
	kubectl get services -o wide

pvc:
	kubectl get pvc

hpa:
	kubectl get hpa