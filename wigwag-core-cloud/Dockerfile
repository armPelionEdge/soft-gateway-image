FROM docker-registry.wigwag.io:5000/ww-ubuntu-base-node
MAINTAINER devops@wigwag.com
ENV PATH=$PATH:/wigwag:/opt/bin
RUN groupadd -g 501 -r wigwag && useradd -r -g wigwag -u 501 -m -d /home/wigwag wigwag && mkdir -p /home/wigwag/.ssh
ADD build-wwcontainer.sh /wigwag
ADD wigwag_devops_cloud_github_latest /wigwag
# add github.com to known_hosts so that git won't go interactive with Yes/No question
RUN chown wigwag:wigwag /wigwag/wigwag_devops_cloud_github_latest && gosu wigwag ssh-keyscan -t rsa github.com > /home/wigwag/.ssh/known_hosts
ADD ssh-config/config /home/wigwag/.ssh
RUN chmod 600 /home/wigwag/.ssh/config && chown wigwag:wigwag /home/wigwag/.ssh/config && chown -R wigwag:wigwag /home/wigwag/.ssh
RUN /wigwag/build-wwcontainer.sh wigwag-cloud
# add the entrypoint script
VOLUME [ "/config", "/apps ]
ADD entry.sh /home/wigwag
RUN chown wigwag:wigwag /home/wigwag/entry.sh && chmod a+x /home/wigwag/entry.sh
ENTRYPOINT [ "/home/wigwag/entry.sh" ]
#CMD /home/wigwag/entry.sh
#CMD /home/wigwag/entry.sh
