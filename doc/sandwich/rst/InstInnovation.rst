+----------------+-----------------+
| InstInnovation | R Documentation |
+----------------+-----------------+

Innovation and Institutional Ownership
--------------------------------------

Description
~~~~~~~~~~~

Firm-level panel data on innovation and institutional ownership from
1991 to 1999 over 803 firms. The observations refer to different firms
over different years.

Usage
~~~~~

::

    data("InstInnovation")

Format
~~~~~~

A data frame containing 6208 observations on 25 variables.

company
    factor. company names.

sales
    numeric. sales (in millions of dollars).

acompetition
    numeric. constant inverse Lerner index.

competition
    numeric. varying inverse Lerner index.

capital
    numeric. net stock of property, plant, and equipment.

cites
    integer. future cite-weighted patents.

precites
    numeric. presample average of cite-weighted patents.

dprecites
    factor. indicates zero precites.

patents
    integer. granted patents.

drandd
    factor. indicates a zero R&D stock.

randd
    numeric. R&D stock (in millions of dollars).

employment
    numeric. employment (in 1000s).

sp500
    factor. membership of firms in the S&P500 index.

tobinq
    numeric. Tobin's q.

value
    numeric. stock market value.

institutions
    numeric. proportion of stock owned by institutions.

industry
    factor. four-digit industry code.

year
    factor. estimation period.

top1
    numeric. share of the largest institution.

quasiindexed
    numeric. share of "quasi-indexed" institutional owners.

nonquasiindexed
    numeric. share of "non-quasi-indexed" institutional owners.

transient
    numeric. share of "transient" institutional owners.

dedicated
    numeric. share of "dedicated" institutional owners.

competition4
    numeric. varying inverse Lerner index in the firm's four-digit
    industry.

subsample
    factor. subsample for the replication of columns 1–5 from Table 4 in
    Aghion et al. (2013).

Details
~~~~~~~

Aghion et al. (2013) combine several firm level panel datasets (e.g.,
USPTO, SEC and Compustat) to examine the role of institutional investors
in the governance of innovation. Their baseline to model innovation is
the Poisson model, but they also consider negative binomial models.
Berger et al. (2017) argue that nonlinearities in the innovation process
emerge in case that the first innovation is especially hard to obtain in
comparison to succeeding innovations. Then, hurdle models offer a useful
way that allows for a distinction between these two processes. Berger et
al. (2017) show that an extended analysis with negative binomial hurdle
models differs materially from the outcomes of the single-equation
Poisson approach of Aghion et al. (2013).

Institutional ownership (institutions) is defined as the proportion of
stock owney by institutions. According to Aghion et al. (2013), an
institutional owner is defined as an institution that files a Form 13-F
with the Securities and Exchange Commission (SEC).

Future cite-weighted patents (cites) are used as a proxy for innovation.
They are calculated using ultimately granted patent, dated by year of
application, and weight these by future citations through 2002 (see
Aghion et al. (2013)).

The presample average of cite-weighted patents (precites) is used by
Aghion et al. (2013) as a proxy for unobserved heterogeneity, employing
the "presample mean scaling" method of Blundell et al. (1999).

The inverse Lerner index in the firm's three-digit industry is used as a
time-varying measure for product market competition (competition), where
the Lerner is calculated as the median gross margin from the entire
Compustat database in the firm's three-digit industry (see Aghion et al.
(2013)). A time-invariant measure for competition (acompetition) is
constructed by averaging the Lerner over the sample period.

The classification of institutions into "quasiindexed", "transient" and
"dedicated" follows Bushee (1998) and distinguishes between
institutional investors based on their type of investing. Quasiindexed
institutions are do not trade much and are widely diversified, dedicated
institution do not trade much and have more concentrated holdings, and
transient institutions often trade and have diversified holdings (see
Aghion et al. (2013) and Bushee (1998)).

Source
~~~~~~

Data and online appendix of Aghion et al. (2013).

References
~~~~~~~~~~

Aghion P, Van Reenen J, Zingales L (2013). “Innovation and Institutional
Ownership.” *The American Economic Review*, **103**\ (1), 277–304. doi:
`10.1257/aer.103.1.277 <http://doi.org/10.1257/aer.103.1.277>`__

Berger S, Stocker H, Zeileis A (2017). “Innovation and Institutional
Ownership Revisited: An Empirical Investigation with Count Data Models.”
*Empirical Economics*, **52**\ (4), 1675–1688. doi:
`10.1007/s00181-016-1118-0 <http://doi.org/10.1007/s00181-016-1118-0>`__

Blundell R, Griffith R, Van Reenen J (1999). “Market Share, Market Value
and Innovation in a Panel of British Manufacturing Firms.” *Review of
Economic Studies*, 66(3), 529–554.

Bushee B (1998). “The Influence of Institutional Investors on Myopic R&D
Investment Behavior.” *Accounting Review*, **73**\ (3), 655–679.

Examples
~~~~~~~~

::

    ## Poisson models from Table I in Aghion et al. (2013)

    ## load data set
    data("InstInnovation", package = "sandwich")

    ## log-scale variable
    InstInnovation$lograndd <- log(InstInnovation$randd)
    InstInnovation$lograndd[InstInnovation$lograndd == -Inf] <- 0

    ## regression formulas
    f1 <- cites ~ institutions + log(capital/employment) + log(sales) + industry + year
    f2 <- cites ~ institutions + log(capital/employment) + log(sales) +
      industry + year + lograndd + drandd
    f3 <- cites ~ institutions + log(capital/employment) + log(sales) +
      industry + year + lograndd + drandd + dprecites + log(precites)

    ## Poisson models
    tab_I_3_pois <- glm(f1, data = InstInnovation, family = poisson)
    tab_I_4_pois <- glm(f2, data = InstInnovation, family = poisson)
    tab_I_5_pois <- glm(f3, data = InstInnovation, family = poisson)

    ## one-way clustered covariances
    vCL_I_3 <- vcovCL(tab_I_3_pois, cluster = InstInnovation$company)
    vCL_I_4 <- vcovCL(tab_I_4_pois, cluster = InstInnovation$company)
    vCL_I_5 <- vcovCL(tab_I_5_pois, cluster = InstInnovation$company)

    ## replication of columns 3 to 5 from Table I in Aghion et al. (2013)
    cbind(coef(tab_I_3_pois), sqrt(diag(vCL_I_3)))[2:4, ]
    cbind(coef(tab_I_4_pois), sqrt(diag(vCL_I_4)))[c(2:4, 148), ]
    cbind(coef(tab_I_5_pois), sqrt(diag(vCL_I_5)))[c(2:4, 148), ]
