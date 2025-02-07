FROM node:22
ADD ./entrypoint.sh /root/entrypoint.sh
RUN yarn global add pm2 hydrooj @hydrooj/ui-default
RUN yarn global add @hydrooj/blog
RUN hydrooj addon add @hydrooj/blog
RUN yarn global add @hydrooj/import-qduoj
RUN hydrooj addon add @hydrooj/import-qduoj
RUN yarn global add @hydrooj/onsite-toolkit
RUN hydrooj addon add @hydrooj/onsite-toolkit
RUN mkdir /root/public
RUN hydrooj addon create
RUN rm /root/addon/public -r
RUN ln -s /root/public /root/addon/public
RUN rm /root/.hydro/addons/addon/public -r
RUN ln -s /root/public /root/.hydro/addons/addon/public

RUN chmod +x /root/entrypoint.sh && \
    mkdir -p /root/.hydro
ENTRYPOINT /root/entrypoint.sh
