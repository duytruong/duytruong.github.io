---
layout: post
title: ﻿Monitor các Docker service với Prometheus
---


Monitor là một việc làm bắt buột trong môi trường production nếu muốn biết tình trạng của hệ thống hoặc các vấn đề đang xảy ra để có biện pháp giải quyết sự cố. Bài viết này sẽ trình bày cách monitor các thành phần (các Docker service) của một hệ thống chạy trong [Docker Swarm](https://docs.docker.com/engine/swarm/) cluster bằng [Prometheus](https://prometheus.io/).

Trong trường hợp bình thường khi các Docker service chỉ chạy với một instance, việc monitor có thể được thực hiện như trong tài liệu của Prometheus. Nhưng khi muốn scale out các service ra thành nhiều instance, ta phải cấu hình để Prometheus có thể tự động "thấy" được các instance hiện có và scrape các metric từ chúng.

Giả sử đã có một Swarm cluster được dựng sẵn, tạo một [overlay network](https://docs.docker.com/engine/userguide/networking/get-started-overlay/), các service đã được deploy, attach vào network đã tạo và có thể scale ra nhiều instace (dùng lệnh `docker service scale service_name=5` hoặc khai báo trong file `docker-compose.yml`).

Tiếp theo, trong file `prometheus.yml`, cấu hình các target cần scrape bằng cơ chế [DNS service discovery](https://prometheus.io/docs/operating/configuration/#<dns_sd_config>)

{% highlight yaml %}
scrape_configs:
  - job_name: 'your-job-name'
    dns_sd_configs:
      - names: ['tasks.service-name']
        type: A
        port: 7071
{% endhighlight %}

Lưu ý, service-name phải giống với name của Docker service cần monitor.

Với cấu hình như vậy, Prometheus sẽ tự resolve được IP của các service (IP trong overlay network) và lấy được các metric cần monitor.

### Tham khảo

- [https://github.com/juliusv/prometheus_docker_demo](https://github.com/juliusv/prometheus_docker_demo)
- [Monitoring, the Prometheus Way](https://www.youtube.com/watch?v=PDxcEzu62jk)
