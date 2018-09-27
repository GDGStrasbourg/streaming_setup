FROM tiangolo/nginx-rtmp

COPY nginx.conf /etc/nginx/nginx.conf
COPY rtmp.conf /etc/nginx/rtmp.conf
COPY stream /usr/bin/stream

RUN chmod +x /usr/bin/stream
