FROM rubydata/notebooks:latest

USER root

RUN apt-get update && apt-get install -y --no-install-recommends unzip
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - && \
    apt-get update && apt-get install -y --no-install-recommends nodejs
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y --no-install-recommends yarn
RUN chown -R $NB_USER $GEM_HOME

USER $NB_USER

RUN mkdir -p $HOME/data/mnist && cd $HOME/data/mnist \
    && wget -O mnist.zip http://data.mxnet.io/mxnet/data/mnist.zip
RUN mkdir -p $HOME/data/cifar10 && cd $HOME/data/cifar10 \
    && wget -O cifar10.zip http://data.mxnet.io/mxnet/data/cifar10.zip
RUN mkdir -p $HOME/data/cifar100 && cd $HOME/data/cifar100 \
    && wget -O cifar100.zip http://data.mxnet.io/mxnet/data/cifar100.zip

RUN cd $HOME/data/mnist \
    && unzip -x mnist.zip \
    && rm -f mnist.zip
RUN cd $HOME/data/cifar10 \
    && unzip -x cifar10.zip \
    && mv cifar/* . \
    && rmdir cifar \
    && rm -f cifar10.zip
RUN cd $HOME/data/cifar100 \
    && unzip -x cifar100.zip \
    && rm -f cifar100.zip

RUN git clone https://github.com/mrkn/mxnet.rb $HOME/mxnet.rb && \
    cd mxnet.rb && \
    bundle install && \
    rake compile

RUN git clone https://github.com/RubyData/wine_data_viewer.git $HOME/wine_data_viewer && \
    cd wine_data_viewer && \
    bundle install && \
    rake db:create db:migrate db:seed

RUN git clone https://github.com/hatappi/rails-red-chainer $HOME/rails-red-chainer && \
    cd rails-red-chainer && \
    bundle install && \
    yarn install
