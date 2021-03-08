# app-insights-event-action
This is a very basic [composite run steps GitHub Action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-run-steps-action) that allows you to send events to Azure Application Insights. Each event will be sent with some [default GitHub environment variables](https://docs.github.com/en/actions/reference/environment-variables#default-environment-variables). Full example usage of this action can be seen in [this repository](https://github.com/syedhassaanahmed/test-app-insights-event-action).

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

Here is an example of measuring duration between above 2 events in App Insights
```sql
let events = customEvents
| project timestamp, name, run_number = toint(customDimensions.run_number);
let begins = events | where name == "begin-provision-azure" | project-rename beginTime = timestamp;
let ends = events | where name == "end-provision-azure" | project-rename endTime = timestamp;
begins | join ends on run_number
| project run_number, durationInSeconds = datetime_diff("second", endTime, beginTime)
| order by run_number
| render columnchart
```