---
layout: post
title: Note về chuyện kết nối tới PostgreSQL
---


Gần đây mình có tạo một Spring-Boot app connect tới PostgreSQL (tất cả đều nắm trên máy local). Lúc chạy thì bị báo lỗi không login được


> org.postgresql.util.PSQLException: FATAL: password authentication failed for user "username"

Username và password cấu hình trong app là username và password của tài khoản Ubuntu mà mình login vào máy. Khi login vào PostgreSQL bằng lệnh
`psql -d dbname` thì được bình thường. Sau một hồi tìm hiểu thì cũng biết được lý do.

PostgreSQL hỗ trợ nhiều phương thức login<sup>[[1]](https://www.postgresql.org/docs/current/static/auth-methods.html)</sup>: Trust, Peer, Password, LDAP,... Khi login bằng lệnh `psql -d dbname` PostgreSQL sẽ kết nối thông qua Unix socket sử dụng Peer Authentication, phương pháp này sử dụng username của hệ điều hành như là database username để xác thực<sup>[[2]](https://www.postgresql.org/docs/current/static/auth-methods.html#AUTH-PEER)</sup>. Do đó, nếu tạo 1 user khác trong PostgreSQL khác với username của hệ điệu hành thì khi login bằng `psql -U new_username -d dbname` sẽ gặp lỗi `psql: FATAL:  Peer authentication failed for user "new_username"`.

Để app kết nối được với PostgreSQL, mình phải đặt password cho username dùng để login (password này khác với password của hệ điều hành). Lúc này Password Authentication sẽ được sử dụng, lưu ý nếu không đặt password thì không login được nhé.
