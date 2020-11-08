RANGE = $(shell seq 2 1834)

all: $(addsuffix .good,$(RANGE)) $(addsuffix .bad,$(RANGE))
	cat *.good > all.good
	cat *.bad > all.bad

%.html:
	curl "http://apguve.korpuss.lv/taskreport/$(basename $@)/edit" \
		-H 'Connection: keep-alive' \
		-H 'Cache-Control: max-age=0' \
		-H 'Upgrade-Insecure-Requests: 1' \
		-H 'User-Agent: Mozilla/5.0 (X11; Fedora; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.111 Safari/537.36' \
		-H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9' \
		-H 'Accept-Language: en-US,en;q=0.9,es-US;q=0.8,es;q=0.7' \
		-H 'Cookie: _ga=GA1.2.1158147799.1603239562; _gid=GA1.2.1556837741.1604862626; PHPSESSID=n8mgqbvo3kf3cqehr6hmo8bbtj' \
		--compressed --fail \
		--insecure > ./$@

%.good: %.html
	sed -e 's/\(<[^>]*>\)/\1\n/g' $< | sed -n '/Correct text/,/\/textarea/p' | sed -e 's/<[^>]*>//g' | grep -v "Correct text" | tr '\r\n' ' ' > $@ || echo None
	echo >> $@

%.bad: %.html
	sed -e 's/\(<[^>]*>\)/\1\n/g' $< | sed -n '/Text/,/Correct text/p' | sed -e 's/<[^>]*>//g' | grep -v "Text\|Correct text" | tr '\r\n' ' ' > $@ || echo None
	echo >> $@
