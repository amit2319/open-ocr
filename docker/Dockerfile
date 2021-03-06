FROM ubuntu

ENV GOPATH /opt/go

  # get the software proprerties common golang gcc and git
  # We need to install the software properties common before doing add-apt-repository, otherwise it will give an error: add-apt-repository: not found

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=America/New_York

RUN apt-get update && apt-get install -y \
software-properties-common \
git \
golang \
gcc \
curl

RUN add-apt-repository ppa:alex-p/tesseract-ocr && apt-get update

  # Get tesseract-ocr packages
RUN apt-get install -y \
libleptonica-dev \
libtesseract4 \
libtesseract-dev \
tesseract-ocr

  # Get language data.
RUN curl -LO https://github.com/tesseract-ocr/tessdata_best/raw/master/eng.traineddata && \
mv eng.traineddata /usr/share/tesseract-ocr/4.00/tessdata/

RUN curl -LO https://github.com/BigPino67/Tesseract-MICR-OCR/raw/master/training/tessdata/mcr.traineddata && \
mv mcr.traineddata /usr/share/tesseract-ocr/4.00/tessdata/

RUN curl -LO https://github.com/tesseract-ocr/tessdata/raw/3.04.00/equ.traineddata && \
mv equ.traineddata /usr/share/tesseract-ocr/4.00/tessdata/

RUN curl -LO https://github.com/Shreeshrii/tessdata_ocrb/raw/master/ocrb.traineddata && \
mv ocrb.traineddata /usr/share/tesseract-ocr/4.00/tessdata/

RUN mkdir -p $GOPATH

  # go get open-ocr
RUN go get -u -v -t github.com/amit2319/open-ocr

  # build open-ocr-httpd binary and copy it to /usr/bin
RUN cd $GOPATH/src/github.com/amit2319/open-ocr/cli-httpd && go build -v -o open-ocr-httpd && cp open-ocr-httpd /usr/bin

  # build open-ocr-worker binary and copy it to /usr/bin
RUN cd $GOPATH/src/github.com/amit2319/open-ocr/cli-worker && go build -v -o open-ocr-worker && cp open-ocr-worker /usr/bin
