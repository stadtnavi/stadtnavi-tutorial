ARG OTP_TAG=latest
ARG OTP_IMAGE=mfdz/opentripplanner

FROM $OTP_IMAGE:$OTP_TAG AS otp

# defined empty, so we can access the arg as env later again
ARG OTP_VERSION
ENV ROUTER_NAME=default
ARG gtfs_url=https://www.openvvs.de/dataset/e66f03e4-79f2-41d0-90f1-166ca609e491/resource/bfbb59c7-767c-4bca-bbb2-d8d32a3e0378/download/google_transit.zip
ENV GTFS_URL=$gtfs_url
ARG osm_pbf_url=https://download.geofabrik.de/europe/germany/baden-wuerttemberg/stuttgart-regbez-latest.osm.pbf
ENV OSM_PBF_URL=$osm_pbf_url
ARG memory=31G
ENV MEMORY=$memory

RUN apk add --update zip && \
    rm -rf /var/cache/apk/*

RUN mkdir -p /opt/opentripplanner/build/$ROUTER_NAME/

# add build data
# NOTE: we're using dockers caching here. Add items in order of least to most frequent changes
ADD router-config.json /opt/opentripplanner/build/$ROUTER_NAME/
ADD build-config.json /opt/opentripplanner/build/$ROUTER_NAME/
ADD $OSM_PBF_URL /opt/opentripplanner/build/$ROUTER_NAME/
ADD $GTFS_URL /opt/opentripplanner/build/$ROUTER_NAME/

# print version
RUN java -jar otp-shaded.jar --version | tee build/version.txt
RUN echo "image: mfdz/opentripplanner:$OTP_VERSION" >> build/version.txt

# build
RUN java -Xmx$MEMORY -jar otp-shaded.jar --build build/$ROUTER_NAME | tee build/build.log

# package: graph and config into zip
RUN sh -c 'cd /opt/opentripplanner/build/; export VERSION=$(grep "version:" version.txt | cut -d" " -f2); zip graph-$ROUTER_NAME-$VERSION.zip $ROUTER_NAME/Graph.obj $ROUTER_NAME/router-*.json'

RUN rm -rf /opt/opentripplanner/build/$ROUTER_NAME

# ---

FROM nginx:alpine

RUN sed -i 'N; s/index  index.html index.htm;/autoindex on;/' /etc/nginx/conf.d/default.conf; \
    sed -i '/error_page/d' /etc/nginx/conf.d/default.conf
RUN rm /usr/share/nginx/html/*.html

COPY --from=otp /opt/opentripplanner/build/ /usr/share/nginx/html/