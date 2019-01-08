# Getting started with Oracle OpenSSL FIPS

## Overview

The Oracle OpenSSL FIPS is based on the OpenSSL FIPS Object Module
version 2.0.13 with the following enhancement:

    - HW crypto support
      Add support for AESNI GCM and SPARC (montmul/AES/DES/SHA)
      HW acceleration.  The changes are just brought over
      from OpenSSL 1.0.2  (No new code development)

    - FIPS 186-4: RSA key generation
      Make OpenSSL FOM conform to FIPS186-4

    - IG 9.10 requirement
      The FOM module always executes the integrity test every time
      it is loaded.  POST is executed only if FIPS mode is enabled.
      If the system defines a function '_fips_config_check()',
      the function is called to check the config file and determine
      whether FIPS mode is enabled or disabled.
      If the function is not defined, FIPS mode is enabled by default.

      Note:
      FIPS mode is always enabled for Solaris and ZFSSA.
      ILOM provides the function _fips_config_check().

    - IG 9.11 [ILOM only]
      If FIPS mode is enabled, run the POST once and cache the
      POST result in shared memory (only writable by root).
      All other processes just run the integrity test.

    - Multi-threading issue
      The API allows users to specify the threadid callback
      functions, and that could lead the application to
      reference an invalid memory (an old threadid callback
      function)  To avoid the issue, such API is disabled,
      and use pthread_self() to identify the thread.

The main consumer of the Oracle OpenSSL FIPS Object Module (FOM) is

    - Oracle Solaris server systems
    - Oracle ZFSSA
    - Oracle ILOM

The Oracle OpenSSL FOM is developed in the Oracle Userland project:

	https://github.com/oracle/solaris-userland/

under the 'fipscanister-dev' component dir:

	components/openssl/openssl-fips-140/fipscanister-dev

## Building the Oracle OpenSSL FIPS Object Module (FOM)

The detail description of the build instruction can be found in the Oracle
FOM Security Policy[1].

That compiled module is NOT FIPS 140-2 validated or suitable for use in
satisfying a requirement for the use of FIPS 140-2 validated cryptography
UNLESS the requirements of the Security Policy are followed exactly.

## Reference

[1] https://github.com/oracle/solaris-openssl-fips/releases/download/v1.0/Oracle.OpenSSL.FOM.Security.Policy.Cert.3335.pdf


