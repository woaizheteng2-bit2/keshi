# 课时费计算器

这是一个可部署到 GitHub Pages 的前端网站，支持本地保存和 Supabase 云同步。

## 运行方式

1. 把仓库部署到 GitHub Pages
2. 打开网站后进入“设置”
3. 填入你的 Supabase 项目：
   - 项目 URL
   - anon key
   - 登录邮箱
   - 登录密码
4. 点击“连接云端”

## Supabase 建表

在 Supabase 的 SQL Editor 里执行：

```sql
create table if not exists public.workbuddy_state (
  user_id uuid primary key,
  records jsonb not null default '[]'::jsonb,
  students jsonb not null default '{}'::jsonb,
  classes jsonb not null default '{}'::jsonb,
  updated_at timestamptz not null default now()
);

alter table public.workbuddy_state enable row level security;

create policy "Users can manage their own state"
on public.workbuddy_state
for all
to authenticated
using (auth.uid() = user_id)
with check (auth.uid() = user_id);
```

## 说明

- 网站代码放在 GitHub
- 数据放在 Supabase
- 同一个邮箱账号在任何电脑登录都能看到同一份数据
- 记得偶尔用“导出备份”再留一份本地备份
