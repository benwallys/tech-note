**users 表**

uid | nick | avatar | sex
--- | --- | --- | ---
1 | Tomy | face001 | 1
2 | Jacy | face002 | 2
3 | Prod | face003 | 2
4 | Gooden | face004 | 1
5 | Young | face005 | 1

**groups 表**

gid | gname | uid | in_group
--- | --- | --- | ---
1 | group001 | 1 | 1
2 | group001 | 2 | 1
3 | group002 | 1 | 0
4 | group001 | 3 | 0
5 | group001 | 4 | 1
6 | group001 | 5 | 1
7 | group002 | 3 | 1
8 | group002 | 5 | 1



### 目标：查询某个 gid 内 in_group = 1 的用户的资料


```
// MySQL 原生语句
SELECT
    users.uid,
    users.nick,
    users.avatar,
    users.sex
FROM
    groups,
    users
WHERE
    groups.uid = users.uid
AND groups.gname = 'group001'
AND groups.in_group = 1
ORDER BY
    users.uid DESC
LIMIT 3
```

### Laravel 中使用查询构造器（QueryBuilder）的做法

```
$users = DB::table('groups')
            ->join('users', 'groups.uid', '=', 'users.uid')
            ->select('users.uid', 'users.nick', 'users.avatar', 'users.sex')
            ->where('groups.gname', 'group001')
            ->where('groups.in_group', 1)
            ->orderBy('users.uid', 'desc')
            ->limit(3)
            ->get();
```
