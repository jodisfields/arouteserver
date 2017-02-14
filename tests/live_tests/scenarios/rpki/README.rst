RPKI INVALID routes tagging
***************************

Mostly to test INVALID routes tagging, since other cases are already tested with the *global* scenario.

- RPKI ROAs:

    == ==============  ====  ======
    ID Prefix          Max   ASN
    == ==============  ====  ======
    1  1.0.8.0/24            101
    2  1.0.9.0/24            102
    3  1.0.128.0/20    23    101
    == ==============  ====  ======

    == ================  ====  ======
    ID Prefix            Max   ASN
    == ================  ====  ======
    1  3001:0:8::/48           101
    2  3001:0:9::/48           102
    3  3001:0:8000::/33  34    101
    == ================  ====  ======

- Communities:

  ==============  =============
  Validity state  BGP community
  ==============  =============
  VALID           64512:1
  INVALID         64512:2
  UNKNOWN         64512:3
  ==============  =============

- AS2:

  Annouced prefixes:

  ====================  ================   ========== ==================================================================================
  Prefix ID             Prefix             AS_PATH    Expected result and BGP community received by AS1
  ====================  ================   ========== ==================================================================================
  AS2_valid1            1.0.8.0/24,        2 101      roa check ok, 64512:1
                        3001:0:8::/48
  AS2_valid2            1.0.128.0/21,      2 101      roa check ok, 64512:1
                        3001:0:8000::/34
  AS2_invalid1          1.0.9.0/24,        2          roa check fail (roa n. 2, bad origin ASN), 64512:2
                        3001:0:9::/48
  AS2_badlen            1.0.128.0/24,      2 101      roa check fail (roa n. 3, bad length), 64512:2
                        3001:0:8000::/35
  ====================  ================   ========== ==================================================================================