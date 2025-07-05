```yaml
trigger: [ "今日のTODO", "今日のタスク" ]
objective: "秘書として今日もしくは明日のTODOリストの作成を行います。"
steps:

    - if: not ${current_date}

      - name: "init_date"
        action: "call"
        target: "date_init.md"

    - name: "read_todo_task_files"
      action: "read_files"
      path: [ "todo/${prev_date}_todo.md", "task/routine.md", "task/long_term.md", "task/short_term.md" ]
      message: "前営業日のTODOリスト、各タスクを読み込みました。"
      
    - name: "pickup_incomplete_tasks"
      action: "filter_tasks"
      source: "todo/${prev_date}_todo.md"
      message: "前営業日の未完了タスクを抽出しました。"

    - name: "pickup_routine_tasks"
      action: "filter_tasks"
      source: "task/routine.md"
      filter: "${today_weekday}にマッチするタスク"
      message: "本日実施すべきルーティンワークを抽出しました。"

    - name: "create_todays_task"
      action: "create_file"
      template: "todo/template_todo.md"
      dest: "todo/${current_date}_todo.md"
      message: "本日のTODOリストを作成しました。"

    - name: "create_todays_memo"
      action: "create_file"
      template: "memo/template_memo.md"
      dest: "memo/${current_date}_memo.md"
      note: "会議メモには会議ごとの小見出しを追加"
      message: "本日のメモを作成しました。"

    - name: "confirm_another_task" 
      action: "confirm"
      message: "他に追加するタスクがあればお申し付けください。"

```