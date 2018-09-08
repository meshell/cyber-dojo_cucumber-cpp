ARG GCC_VERSION=8.2.0
FROM  gcc:${GCC_VERSION}
LABEL maintainer=estermann.michel@gmail.com

ARG CUKE_VERSION=v0.5

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y ruby ruby-dev
RUN gem install bundler

# Install (build) googletest and boost
RUN apt-get install -y cmake git libboost-all-dev libgtest-dev
WORKDIR /usr/src/gtest
RUN cmake .
RUN make
RUN mv libg* /usr/lib

# build cucumber-cpp
WORKDIR /
RUN git clone https://github.com/cucumber/cucumber-cpp.git
WORKDIR "cucumber-cpp"
RUN git checkout tags/${CUKE_VERSION}

RUN bundle install
RUN mkdir build && (cd build && cmake -DCUKE_DISABLE_QT=on -DCUKE_ENABLE_EXAMPLES=on ..)
RUN cmake --build build --target install

COPY red_amber_green.rb /usr/local/bin
WORKDIR /

# cleanup
RUN rm -rf cucumber-cpp
RUN apt-get remove -y cmake
