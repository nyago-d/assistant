```yaml
steps:

  - name: "get_current_date"
    action: "execute_shell"
    command: "Get-Date -Format \"yyyy-MM-dd\""
    variable: "current_date"
    message: "現在の日付を取得しています..."

  - name: "get_weekday"
    action: "execute_shell"
    command: "(Get-Date).DayOfWeek"
    variable: "today_weekday"

  - name: "confirm_current_date"
    action: "think_prev_working_day"
    variable: "prev_date"
    message: "現在の日付は${current_date}（${today_weekday}）です。前営業日は${prev_date}です。"

```