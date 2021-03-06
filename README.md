# Semver.sh

Semver.sh is a library of semantic versioning functions written in pure Bash,
dual licensed under the Apache License 2.0 and MIT Licenses, at your discretion.
It can be safely sourced into your script or copied in piecemeal (see VENDORING
in the manual).

To test, execute `test.sh`. Building the full README or traditional manual page
requires [**mandoc(1)**](http://mandoc.bsd.lv/).

# SEMVER(3)


<div class="manual-text">
<h2 class="Sh" id="NAME" title="Sh">NAME</h2>
<b class="Nm" title="Nm">Semver</b> &mdash;
  <span class="Nd" title="Nd">semantic versioning functions</span>
<h2 class="Sh" id="SYNOPSIS" title="Sh">SYNOPSIS</h2>
<table class="Nm">
  <tr>
    <td><b class="Nm" title="Nm">Semver::validate</b></td>
    <td><var class="Ar" title="Ar">version</var>
      [<span class="Op"><var class="Ar" title="Ar">variable</var></span>]</td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::compare</b></td>
    <td><var class="Ar" title="Ar">version</var>
      <var class="Ar" title="Ar">version</var></td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::is_prerelease</b></td>
    <td><var class="Ar" title="Ar">version</var></td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::increment_major</b></td>
    <td><var class="Ar" title="Ar">version</var></td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::increment_minor</b></td>
    <td><var class="Ar" title="Ar">version</var></td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::increment_patch</b></td>
    <td><var class="Ar" title="Ar">version</var></td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::set_pre</b></td>
    <td><var class="Ar" title="Ar">version</var>
      <var class="Ar" title="Ar">pre-release</var></td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::set_build</b></td>
    <td><var class="Ar" title="Ar">version</var>
      <var class="Ar" title="Ar">build metadata</var></td>
  </tr>

  <tr>
    <td><b class="Nm" title="Nm">Semver::pretty</b></td>
    <td><var class="Ar" title="Ar">major</var>
      <var class="Ar" title="Ar">minor</var>
      <var class="Ar" title="Ar">patch</var>
      [<span class="Op"><var class="Ar" title="Ar">pre-release</var></span>]
      [<span class="Op"><var class="Ar" title="Ar">build_metadata</var></span>]</td>
  </tr>
</table><h2 class="Sh" id="DESCRIPTION" title="Sh">DESCRIPTION</h2>
<b class="Nm" title="Nm">Semver::validate</b> validates a semantic version
  string and optionally returns the parsed output. It prints its input
  <var class="Ar" title="Ar">version</var>, erroring with a description of
  what&#39;s wrong if it&#39;s invalid. If <var class="Ar" title="Ar">variable</var> is
  also provided, a snippet for array assignment of the parsed parts to
  <var class="Ar" title="Ar">variable</var> is instead printed, allowing for an
  easy <b class="Nm" title="Nm">eval</b> afterwards. (You probably want to run
  <code class="Li">declare -a my_var</code> beforehand so static
  analysis tools like <b class="Xr" title="Xr">shellcheck(1)</b> can work
  properly.)
<div class="Pp"></div>
<b class="Nm" title="Nm">Semver::compare</b> compares two semvers and returns
  the result. The result is one of <code class="Li">-1</code>,
  meaning less than, <code class="Li">0</code>, meaning equals,
  or <code class="Li">1</code>, meaning greater than. This
  allows for natural use with <b class="Nm" title="Nm">[[</b> or
  <b class="Xr" title="Xr">test(1)</b>, such as <code class="Li">[[
  $(Semver::compare a b) -ge 0 ]]</code> testing whether
  <var class="Ar" title="Ar">a</var> is greater than or equal to
  <var class="Ar" title="Ar">b</var>.
<div class="Pp"></div>
<b class="Nm" title="Nm">Semver::is_prerelease</b> checks whether a semver is a
  pre-release. The result is either <code class="Li">yes</code>
  or <code class="Li">no</code>.
<div class="Pp"></div>
<b class="Nm" title="Nm">Semver::increment_major</b>,
  <b class="Nm" title="Nm">Semver::increment_minor</b>, and
  <b class="Nm" title="Nm">Semver::increment_patch</b> return a new semver with
  the relevant part incremented. This does not modify
  <var class="Ar" title="Ar">version</var>. When incrementing, the pre-release
  and build metadata parts are dropped.
<div class="Pp"></div>
<b class="Nm" title="Nm">Semver::set_pre</b> and
  <b class="Nm" title="Nm">Semver::set_build</b> return a new semver with the
  relevant part replaced. This does not modify
  <var class="Ar" title="Ar">version</var>. Validation is performed on
  <var class="Ar" title="Ar">part</var>. A <var class="Ar" title="Ar">part</var>
  of the empty string removes the part. Setting the pre-release does not drop
  the build metadata.
<div class="Pp"></div>
<b class="Nm" title="Nm">Semver::pretty</b> takes separate parts and formats
  them into a semver. Note that no validation is performed on the returned
  version.
<h2 class="Sh" id="VENDORING" title="Sh">VENDORING</h2>
If you wish to copy and paste these functions into your own script to vendor
  them, <b class="Nm" title="Nm">Semver::validate</b> &amp;
  <b class="Nm" title="Nm">Semver::pretty</b> have no dependencies. All other
  functions depend on <b class="Nm" title="Nm">Semver::validate</b>. Please
  remember to provide proper attribution and license information.
<h2 class="Sh" id="ERRORS" title="Sh">ERRORS</h2>
All errors start with the name of their associated function, like so:
<div class="D1"><code class="Li">Semver::validate: ...</code></div>
<dl class="Bl-tag">
  <dt class="It-tag"><b class="Nm" title="Nm">Semver::validate</b></dt>
  <dd class="It-tag">
    <ul class="Bl-bullet">
      <li class="It-bullet"><code class="Li">invalid major:
          $major</code> | <code class="Li">inavlid minor:
          $minor</code> | <code class="Li">invalid patch:
          $patch</code>
        <div class="Pp"></div>
        The part is not an integer.</li>
      <li class="It-bullet"><code class="Li">invalid pre-release:
          $pre_release</code> | <code class="Li">invalid build
          metadata: $build_metadata</code>
        <div class="Pp"></div>
        The part is not a series of non-empty identifiers
          (<code class="Li">[0-9A-Za-z-]</code>) separated by
          dots (<code class="Li">.</code>).</li>
    </ul>
  </dd>
  <dt class="It-tag"><b class="Nm" title="Nm">Semver::compare</b></dt>
  <dd class="It-tag">
    <ul class="Bl-bullet">
      <li class="It-bullet">All <b class="Nm" title="Nm">Semver::validate</b>
          errors.</li>
    </ul>
  </dd>
  <dt class="It-tag"><b class="Nm" title="Nm">Semver::increment_major</b> |
    <b class="Nm" title="Nm">Semver::increment_minor</b> |
    <b class="Nm" title="Nm">Semver::increment_patch</b></dt>
  <dd class="It-tag">
    <ul class="Bl-bullet">
      <li class="It-bullet">All <b class="Nm" title="Nm">Semver::validate</b>
          errors.</li>
    </ul>
  </dd>
  <dt class="It-tag"><b class="Nm" title="Nm">Semver::set_pre</b> |
    <b class="Nm" title="Nm">Semver::set_build</b></dt>
  <dd class="It-tag">
    <ul class="Bl-bullet">
      <li class="It-bullet">All <b class="Nm" title="Nm">Semver::validate</b>
          errors.</li>
      <li class="It-bullet"><b class="Nm" title="Nm">Semver::set_pre</b>:
          <code class="Li">invalid pre-release:
          $pre_release</code> |
          <b class="Nm" title="Nm">Semver::set_build</b>:
          <code class="Li">invalid build metadata:
          $build_metadata</code>
        <div class="Pp"></div>
        The part is not a series of non-empty identifiers
          (<code class="Li">[0-9A-Za-z-]</code>) separated by
          dots (<code class="Li">.</code>).</li>
    </ul>
  </dd>
</dl>
<h2 class="Sh" id="EXAMPLE" title="Sh">EXAMPLE</h2>
The following script is designed to be placed next to a copy of
  <b class="Nm" title="Nm">Semver.sh</b>.
<div class="Pp"></div>

```
#!/bin/bash 
# example.sh 
 
ver_a='1.2.11-alpha+001' 
ver_b='1.4.0' 
ver_invalid='1.5.' 
 
Semver::validate "$ver_invalid" 2>/dev/null \ 
  && echo "$ver_invalid should be an invalid semver" \ 
  || echo "$ver_invalid is an invalid semver" 
 
echo 
 
declare -a v 
eval "$(Semver::validate "$ver_a" v)" 
echo "Parts of \$ver_a:" 
echo "major=${v[0]}, minor=${v[1]}, patch=${v[2]}," 
echo "pre-release=${v[3]}, build metadata=${v[4]}" 
 
echo 
 
compare_vers() { 
  echo -n "Comparsion between $ver_a & $ver_b: " 
  Semver::compare $ver_a $ver_b 
} 
compare_vers 
ver_a=$(Semver::increment_minor "$ver_a") 
ver_a=$(Semver::increment_minor "$ver_a") 
compare_vers 
ver_a=$(Semver::increment_patch "$ver_a") 
compare_vers 
ver_b=$(Semver::increment_patch "$ver_b") 
ver_a=$(Semver::set_pre "$ver_a" 'alpha') 
ver_b=$(Semver::set_pre "$ver_b" 'alpha') 
ver_a=$(Semver::set_build "$ver_a" 'musl') 
ver_b=$(Semver::set_build "$ver_b" 'glibc') 
compare_vers 
ver_b=$(Semver::set_pre "$ver_b" 'beta') 
ver_b=$(Semver::set_build "$ver_b" '') 
compare_vers 
ver_a=$(Semver::set_pre "$ver_a" '') 
ver_a=$(Semver::set_build "$ver_a" '') 
compare_vers 
 
echo 
 
echo -n "Is $ver_b a pre-release? " 
Semver::is_prerelease "$ver_b" 
echo -n "What about $ver_a? " 
Semver::is_prerelease "$ver_a"
```

<div class="Pp"></div>
We can then run <code class="Li">bash example.sh</code> for the
  following output:
<div class="Pp"></div>

```
1.5. is an invalid semver 
 
Parts of $ver_a: 
major=1, minor=2, patch=11, 
pre-release=alpha, build metadata=001 
 
Comparsion between 1.2.11-alpha+001 & 1.4.10: -1 
Comparsion between 1.4.0 & 1.4.0: 0 
Comparsion between 1.4.1 & 1.4.0: 1 
Comparsion between 1.4.1-alpha+musl & 1.4.1-alpha+glibc: 0 
Comparsion between 1.4.1-alpha+musl & 1.4.1-beta: -1 
Comparsion between 1.4.1 & 1.4.1-beta: 1 
 
Is 1.4.1-beta a pre-release? yes 
What about 1.4.1? no
```

<h2 class="Sh" id="BUGS" title="Sh">BUGS</h2>
Hopefully not, but if you do find some, let me know on GitHub or email me.
<h2 class="Sh" id="SEE_ALSO" title="Sh">SEE
  ALSO</h2>
<a class="Lk" href="https://semver.org/" title="Lk">Semantic Versioning
  specification</a>, <b class="Xr" title="Xr">bash(1)</b>
<h2 class="Sh" id="AUTHORS" title="Sh">AUTHORS</h2>
<span class="An" title="An">bb010g</span>
  &lt;<a class="Mt" href="mailto:me@bb010g.com" title="Mt">me@bb010g.com</a>&gt;
<div style="height: 1.00em;">&nbsp;</div>
The latest sources, full contributor list, and more can be found at
  <a class="Lk" href="https://github.com/bb010g/semver.sh" title="Lk">https://github.com/bb010g/semver.sh</a>.</div>
<table class="foot">
  <tr>
    <td class="foot-date">June 6, 2018</td>
    <td class="foot-os">1.0.0</td>
  </tr>
</table>
