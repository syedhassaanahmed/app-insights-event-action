# app-insights-event-action
This is a very basic [composite run steps GitHub Action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-run-steps-action) that allows you to send events to Azure Application Insights. Each event will be sent with some [default GitHub environment variables](https://docs.github.com/en/actions/reference/environment-variables#default-environment-variables).

## Usage
```yaml
      - name: "AppInsights: Begin Terraform"
        uses: syedhassaanahmed/app-insights-event-action@main
        with:
          instrumentation-key: "${{ secrets.APP_INSIGHTS_KEY }}"
          event-name: "begin-provision-azure"

      - name: "Terraform Apply"
        run: terraform apply -auto-approve

      - name: "AppInsights: End Terraform"
        uses: syedhassaanahmed/app-insights-event-action@main
        with:
          instrumentation-key: "${{ secrets.APP_INSIGHTS_KEY }}"
          event-name: "end-provision-azure"
```