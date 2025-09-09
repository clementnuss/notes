# Dynamic OCI Regisry (Alvaro Hernandez)

Founder & CEO, OnGres. -> aht.es

OCI images are immutable. which is great, but which also implies you need to
create new image variants when you e.g. want another binary.

for example for postgres, wordpress, .net, the image tagging scheme is complex,
and the number of images is the combinatorial of variants, platforms, runtime,
etc.

## Postgres

there are lots of postgres extensions. (total ~ 1'000 extensions)
how do you pack those extensions in an image?

1. the fatty container (huge image, includes all binaries, most of which unused
   and might introduce security issues). restart time is increased due to the
   size of the image.

2. dynamically inject into container (runtime) - requites some agent to do that

3. generate all combinations of possible container images
   $$\binom{n}{r} = \frac{n!}{r!(n-r)!}$$

4. dynamically generate OCI images

## DOCIR: Dynamic OCI Registry

starting point, all postgres extensions are pacakged as extensions

composes dynamic images on the fly.

runtime.json contains root.diff_ids which contains all hashes of the
uncompressed layers that your are using.

other file that needs to be modified is the manifest itself.
the layers need to be adapted and the config digest as well.

how do you encode the extensions in the image name? use a "URL" scheme:

```
postgres/e/ext1/ext2/ext3
postgres--16.3/e/ext1--v1/ext2--v2
```

demo:

```
42.pga.sh/pga/postgres:latest
42.pga.sh/pga/postgres--16.3/e/rum:latest
42.pga.sh/pga/postgres--16.3/e/rum/ltree/citus/mimeo
```

### what is it?

DOCIR is written in modern Java (21) + Quarkus
