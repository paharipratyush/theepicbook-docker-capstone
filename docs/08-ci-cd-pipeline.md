# Phase 8: CI/CD Pipeline Implementation

An automated Azure DevOps pipeline was engineered to handle Build and Deploy stages, utilizing Git SHA `$(Build.SourceVersion)` for immutable artifact tagging.

### Deep-Dive: Debugging the Pipeline
Building this pipeline revealed three major production hurdles:

1.  **The Microsoft Free-Tier Bottleneck:** The deployment stage hung indefinitely because free ADO accounts block parallel jobs on `ubuntu-latest`. I solved this by routing the deployment job to a local WSL `SelfHostedPool`.
2.  **The ContainerConfig Crash:** Docker Compose v1.29 crashed when attempting to recreate the App container inline. I updated the pipeline to explicitly run `docker-compose stop app` and `docker-compose rm -f app` before pulling the new image, ensuring a safe rolling update.
3.  **The `stderr` Trap:** Azure DevOps falsely flagged the deployment as a failure because Docker outputs standard progress logs ("Pulling...", "Extracting...") to the `stderr` stream. I fixed this by appending `2>&1` to the commands and setting `failOnStdErr: false` in the ADO YAML, forcing the pipeline to recognize the successful deployment.
