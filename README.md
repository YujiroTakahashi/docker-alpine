Alpine リナックス
======================

リポジトリURL
------

* [PHP7パッケージ](http://nl.alpinelinux.org/alpine/edge/testing/)

---


AWSキャッシュ設定
------

````
[memcached]
; Use session locking
; valid values: On, Off
; the default is On
memcached.sess_locking = On

; Session spin lock retry wait time in microseconds.
; Be carefull when setting this value.
; Valid values are integers, where 0 is interpreted as
; the default value. Negative values result in a reduces
; locking to a try lock.
; the default is 150000
memcached.sess_lock_wait = 150000

; memcached session key prefix
; valid values are strings less than 219 bytes long
; the default value is "memc.sess.key."
memcached.sess_prefix = "memc.sess.key."

; memcached session consistent hash mode
; if set to On, consistent hashing (libketama) is used
; for session handling.
; When consistent hashing is used, one can add or remove cache
; node(s) without messing up too much with existing keys
; default is Off
memcached.sess_consistent_hash = Off

; memcached session binary mode
memcached.sess_binary = Off

; memcached session number of replicas
memcached.sess_number_of_replicas = 0

; memcached session replica read randomize
memcached.sess_randomize_replica_read = Off

; Set the compression type
; valid values are: fastlz, zlib
; the default is fastlz
memcached.compression_type = "fastlz"

; Compression factor
; Store compressed value only if the compression
; factor (saving) exceeds the set limit.
;
;  store compressed if:
;    plain_len > comp_len * factor
;
; the default value is 1.3 (23% space saving)
memcached.compression_factor = "1.3"

; The compression threshold
;
; Do not compress serialized values below this threshold.
; the default is 2000 bytes
memcached.compression_threshold = 2000

; Set the default serializer for new memcached objects.
; valid values are: php, igbinary, json, json_array
;
; json - standard php JSON encoding. This serializer
;        is fast and compact but only works on UTF-8
;        encoded data and does not fully implement
;        serializing. See the JSON extension.
; json_array - as json, but decodes into arrays
; php - the standard php serializer
; igbinary - a binary serializer
;
; The default is igbinary if available and php otherwise.
memcached.serializer = "igbinary"

````
---
