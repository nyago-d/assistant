```yaml
trigger: [ "おはようございます", "業務開始" ]
objective: "ユーザの秘書としてタスクの確認等、手助けをします。"
steps:

    - name: "init_date"
      action: "call"
      target: "date_init.md"

    - name: "read_today_files"
      action: "read_files"
      path: [ "todo/${current_date}_todo.md", "memo/${current_date}_memo.md" ]
      variable: "created"
      message: "TODOリスト、タスクを読み込みました。"

    - if: not ${created}

        - name: "create_today_files"
          action: "call"
          target: "create_today_todo.md"
          message: "本日のファイルを作成しました。"

    - name: "confirm_today_tasks"
      action: "confirm_tasks"
      target: "today"

```