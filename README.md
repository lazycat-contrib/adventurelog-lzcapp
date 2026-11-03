# AdventureLog 懒猫微服包

自托管旅行日志：地图上记录行程与地点，为每一站保存照片与笔记。

- 上游：<https://github.com/seanmorley15/AdventureLog>
- 镜像：`ghcr.io/seanmorley15/adventurelog`
- 许可：GPL-3.0（全文见 `content/licenses/LICENSE`）

## 组成

| 服务 | 镜像 | 作用 |
| --- | --- | --- |
| `adventurelog` | `ghcr.io/seanmorley15/adventurelog` | 应用本体，容器内 80 |
| `db` | `postgis/postgis:16-3.5` | PostgreSQL + PostGIS |

## 关键配置说明

- **覆盖上游内置弱口令**：上游 README 写着默认登录 `admin` / `admin`。
  本包用 `DJANGO_ADMIN_USERNAME` / `DJANGO_ADMIN_PASSWORD` / `DJANGO_ADMIN_EMAIL`
  把密码换成部署参数里随机生成的强密码。上游入口脚本
  （`docker/shared/entrypoint-common.sh` 的 `create_superuser_if_needed`）
  只在用户名不存在时创建，因此重启不会重复建号、也不会覆盖你后来改的密码。
- **PostGIS 是必需的**：地点与路线用地理字段存储，普通 PostgreSQL 不够。
- `SITE_URL` 用应用域名，否则应用生成的链接会指向容器内地址。
- DSN/数据库密码用十六进制编码，避免密码里的特殊字符破坏连接串。
- 应用自带账号体系，因此开放 `public_path`，由应用自己鉴权。

## 未验证部分

未在真机上实测：登录、创建行程与地点、上传照片、地图展示、重启后数据留存。
