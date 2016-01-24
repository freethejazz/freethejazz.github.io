---
title: GitHub-Bitbucket Pricing Calculator
layout: post
date: 2016-01-16
description: Pricing calculator for GitHub and Bitbucket
scriptIncludes: github-bitbucket-calculator.js
cssIncludes: github-bitbucket-calculator.css
---
When choosing a place to host your code, many factors come into play, including what kind of RCS you like (git/hg/svn), what sort of community involvement
you foresee, and what the user interface supports. Price is also an important factor. GitHub and Bitbucket have different pricing models, but if you 
complete the sentence in the selection below, you'll see how much you'll pay for each. For more insight into how to make the decision, check out my write-up
on <a href="http://www.infoworld.com/d/application-development/bitbucket-vs-github-which-project-host-has-the-most-227061" target="_blank">GitHub vs. Bitbucket at InfoWorld</a>.

## Cost Comparison

An organization with 
<select id="repos">
<option value="25">0-10</option>
<option value="50">11-20</option>
<option value="100">21-50</option>
<option value="200">51-125</option>
</select>
private repositories and 
<select id="contributors">
<option value="0">0-5</option>
<option value="10">6-10</option>
<option value="25">11-25</option>
<option value="50">26-50</option>
<option value="100">51-100</option>
<option value="200">&gt;100</option>
</select>
contributing developers.

  <div id="results">
    <div class="results-container">
      <div id="winner"></div>
      <div class="one-result">
        <div class="host">Github</div>
        <div class="cost">$<span id="git-cost"></span></div>
      </div>
      <div class="one-result">
        <div class="host">Bitbucket</div>
        <div class="cost">$<span id="bit-cost"></span></div>
      </div>
    </div>
  </div>
