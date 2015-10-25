NAME=statsd
VENDOR=ies
VERSION=0.7.2

PREFIX=/opt/ies
STATSD_USER=_statsd
STATSD_GROUP_=statsd

REPOSITORY=https://github.com/easybiblabs/statsd.git
BRANCH=deploy
CACHE=cache/$(NAME)

UPSTART=$(NAME)/debian/statsd.upstart

.PHONY=package

clean:
	rm -rf cache

build:
	mkdir -p cache/
	if [ ! -d "$(CACHE)" ]; then \
		cd cache \
		&& git clone $(REPOSITORY); \
	else \
		cd $(CACHE) && git pull origin $(BRANCH); \
	fi

install:
	cd $(CACHE) \
	&& npm install --production --no-optional \
	&& npm install node-syslog@1.2.0 \
	&& cd .. \
	&& echo "Patching upstart to use: ${PREFIX}" \
	&& mv "$(UPSTART)" "$(UPSTART).ies" \
	&& (sed -e s~/usr/share~$(PREFIX)~ "$(UPSTART).ies" > "$(UPSTART)") \
	&& rm "$(UPSTART).ies" \

package:
	cd cache && \
fpm -s dir \
--debug \
-t deb \
--name $(NAME) \
--version $(VERSION) \
--iteration $(ITERATION) \
--arch 'all' \
--description 'Debian package for etsy statsd with librato-backend enabled. No NPM required.' \
--prefix $(PREFIX) \
--maintainer 'Imagine Easy Solutions <ops+packaging@imagineeasy.com>' \
--vendor $(VENDOR) \
--url 'https://github.com/easybiblabs/cookery' \
--depends 'nodejs > 0.10' \
--depends 'adduser' \
--deb-user $(STATSD_USER) \
--deb-group $(STATSD_GROUP) \
--deb-suggests 'ies-nodejs' \
--deb-upstart "$(UPSTART)" \
--deb-changelog $(NAME)/debian/changelog \
--after-install ./../postinst \
--after-remove ./../postrm \
--config-files $(PREFIX)/$(NAME)/debian/localConfig.js \
--config-files $(PREFIX)/$(NAME)/debian/proxyConfig.js \
--exclude '**/.git**' \
--exclude '**/.pkgr.yml' \
--exclude '**/.travis.yml' \
--exclude '**/.DS_Store' \
--exclude '**/test/**' \
--exclude '**/tests/**' \
--exclude '**/examples/**' \
--exclude '**/build/**' \
--exclude '**/winser/**' \
./$(NAME)
