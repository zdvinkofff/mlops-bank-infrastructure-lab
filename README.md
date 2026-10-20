# Local MLOps & Bank Infrastructure Sandbox

Локальный демонстрационный стенд для развертывания микросервисной банковской архитектуры и MLOps-контура. Проект воспроизводит промышленную среду для трекинга и изоляции ресурсов ИИ-моделей.

## 🛠 Стек технологий
* **Оркестрация:** Kubernetes (v1.36), Helm 4, Kube-control (kubectl)
* **MLOps:** Python 3, MLflow Server (Локальный трекинг моделей)
* **Зависимости и Хранение:** СУБД PostgreSQL 15, Декларативные манифесты
* **CI/CD & GitOps:** GitLab CI Pipelines, Argo CD Delivery Controller Configuration
* **Observability:** Grafana 10 (Платформенный мониторинг)
* **Базовая среда:** WSL 2 (Ubuntu 24.04 LTS), Docker Desktop

## 🏗 Архитектура проекта
* `/my-app` — Helm-чарт комплексного деплоя (Веб-сервер + PostgreSQL + Grafana + ArgoCD)
* `/my-app/values-stage.yaml` — Конфигурация тестовой среды с квотами ресурсов (CPU 200m / RAM 256Mi)
* `/my-app/values-prod.yaml` — Конфигурация промышленной среды с масштабированием (replicas: 3)
* `/mlops_core` — Изолированный контур для трекинга ИИ-моделей (MLflow)
* `/bank_ci/.gitlab-ci.yml` — Валидированный конвейер автоматизации GitLab (Build + Test)

## 🚦 Инструкция по запуску и проверке работоспособности
1. Запуск Kubernetes и Docker Desktop на хосте Windows.
2. Деплой инфраструктурного пакета: `helm install bank-release ./my-app`
3. Проверка здоровья подов: `kubectl get pods`
4. Проброс шлюза мониторинга: `kubectl port-forward deployment/bank-grafana 3000:3000` (Доступно на http://localhost:3000)
5. Запуск MLOps-сервера: `/home/den/.local/bin/mlflow server --host 127.0.0.1 --port 5000`
