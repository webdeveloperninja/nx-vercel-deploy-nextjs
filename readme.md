# Vercel Deployment Action

This action handles pulling, building, and deploying a Vercel project. It can be used to manage different steps based on the affected apps and a specific app name.

## Inputs

### `affected_apps`
A JSON string that is the result of running `nx show projects --affected --json`. This input determines the apps affected by the current changes and thus the steps that will be executed.

### `vercel_token`
Your Vercel token for authentication.

### `vercel_org_id`
Your Vercel Organization ID.

### `vercel_project_id`
Your Vercel Project ID.

### `app_name`
The specific app name to deploy. Use this input to specify the app you want to deploy within the affected apps.

### `is_production` (Optional)
Set this to `false` if it's not a production build. Defaults to `true`.

## Outputs

### `deployed_url`
URL of the deployed preview app.

### `did_deploy`
A boolean value indicating if the app was deployed. Returns `true` if the app was deployed.

## Example Usage

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      # Other steps here ...

      - name: Vercel Deployment
        id: vercel-deploy
        uses: webdeveloperninja/nx-vercel-deploy-nextjs@main
        with:
          affected_apps: "${{ steps.generate-affected-projects.outputs.affected_projects }}"
          vercel_token: ${{ secrets.VERCEL_TOKEN }}
          vercel_org_id: '9seUj4PGRBp6DIdfhFIbE0XG'
          vercel_project_id: 'prj_QvSp6OnrHNW6J4KAdwqdBC4oBSuS'
          app_name: 'portal'
          is_production: 'true'

      # Use the outputs in the next step
      - name: Print Deployed URL and Deployment Status
        run: |
          echo "Deployed URL: ${{ steps.vercel-deploy.outputs.deployed_url }}"
          echo "Did Deploy: ${{ steps.vercel-deploy.outputs.did_deploy }}"

```


## How to Get affected_apps
You can generate the affected_apps input by running the following command:

`nx affected:apps --plain`
