# Problems using Arquillian Warp with Rewrite

This repository is a fork of the [Arquillian Showcase](https://github.com/arquillian/arquillian-showcase).
Its [rewrite branch](https://github.com/albers/arquillian-showcase/tree/rewrite) illustrates problems I faced when using OCPsoft's URL-Rewriting Framework [rewrite](http://ocpsoft.org/rewrite/) with [Arquillian Warp](http://arquillian.org/modules/warp-extension/).

## Changes made 

1. updated some dependencies in _arquillian-showcase-parent_ to current versions (Arquillian Warp 1.0.0.Alpha4)
2. added a rewrite configuration and a failing test to the _arquillian-showcase-warp_ project

See git changelog for details.

## arquillian-showcase-warp

_arquillian-showcase-warp_ contains two simple JSF applications and a Warp test case for each one of them.

I added `RewriteConfigurationProvider` which introduces an URI alias "/index" for the JSF page "index.xhtml".
In `BasicJSFUnitTestCase`, I added a copy of the existing Warp test.
Now, there are two warp test performing the same task, one using the original URI and one the aliased one.

### Webapp demonstration

To see the URI alias in action,
* cd to the _warp_ directory
* build and deploy to a running local JBoss AS 7.1.1 with `mvn jboss-as:deploy -DskipTests`
* open the JSF page with its original URI [http://localhost:8080/arquillian-showcase-warp/index.jsf](http://localhost:8080/arquillian-showcase-warp/index.jsf)
* open the JSF page with its aliased URI [http://localhost:8080/arquillian-showcase-warp/index](http://localhost:8080/arquillian-showcase-warp/index)
* undeploy with `mvn jboss-as:undeploy`

### Warp demonstration

* Make sure no local JBoss server is running.
* cd to the _warp_ directory
* Run `mvn test`

#### expected result

all tests pass.

#### actual result

`BasicJSFUnitTestCase.shouldExecutePage_usingUrlAlias()` fails with _ClientWarpExecutionException:_ 
_deenriching response failed: The response payload with serialId [...] was never registered_

#### conclusion

The Warp test using the unaliased URI passes as expected.
But when Rewrite rewrites a request, the corresponding Warp test fails.

I'm not sure whether this is an rewrite or Warp issue.
Any help on how to combine these two technologies would be greatly appreciated.
