---
title: "Privacy Measures"
date: 2020-01-15T15:34:30-04:00
categories:
  - Privacy
tags:
  - De-Identification
---


There is a philosophical principle that explains that each event, each living being, each object, each person, or each circumstance has the characteristic of its uniqueness, of its particularity. Other similar events, living beings, objects, persons or circumstances may exist, but never the same entity. This uniqueness is referred to as the objects unicity.

In computer science unicity (&epsilon;<sub>p</sub>) is a risk metric for measuring the re-identifiability of high-dimensional anonymous data. First introduced in 2013, unicity is measured by the number of points p needed to uniquely identify an individual in a data set. The fewer points needed, the more unique the traces are and the easier they would be to re-identify using outside information [[1]].

In the field of data privacy, unicity is usually seen as something that you want to maximize.

The problem with data privacy is that perfectly private information is also perfectly useless. Perfect protection for Bob, would require total exclusion of Bob's information from the analysis. It would also require the exclusion of anyone else's information, in order to provide equal protection for them as well. Not allowing people to do studies on the populace hurts research, and you can only control whether you yourself are in the study, you can't stop other people from participating in the survey even if they are similar to you and you chose to opt-out.

To create useful datasets that can be used by researchers but provide protection there are multiple methods of de-identification that can be used on the dataset. Once these methods have been used tests need to be done to check whether the data is private enough. The two main methods of measurement that are discussed are k-anonymity and differential privacy. These are also sometimes seen as de-identification methods themselves, because of how they control the privacy provided.  

# K-Anonymity
> is satisfied for a dataset in which the identifying attributes for each person are identical to those of at least k − 1 other individuals in the dataset.

Based on the work by Sweeney and Samaratiy [[2],[3]] k-anonymity is a measure of personal information security. A dataset is regarded as k-anonymised if, on all sets of key variables, each combination of possible values of those variables has at least k records that have that same combination of values. The parameter k is defined by the entity carrying out the de-identification, and there are various common choices (3, 5, 10, 15, 100).

The first step in a k-anonymous system is removing or masking direct identifiers. That is the name, address, phone number, email address, etc. Once those are removed or masked there are quasi-identifiers that together can be used to identify people. This is where k-anonymity begins. The quasi-identifiers are attributes that can together be used to identify someone, though on their own they might not be enough.

With k-anonymity the quasi-identifiers need to be adjusted, usually with generalization so that there are always k-1 tuples with the same quasi-identifier values. This can mean changing the age from a set date, to a range of months, or years, or other changes that ensure that there are k-1 matching tuples (when looking at those quasi-identifier attributes) for every combination of values that appear in the table.

By doing this the idea is that you've ensured that no one can be uniquely identified. Even if someone had background knowledge of a person and was trying to find them in the data table, they wouldn't be able to find the exact person that they're looking for, they would at best be able to narrow it down to k-1 entries.

k-anonymity has a few known attacks that will work against it

**Matching Unshuffled Data**: if the table entries are left in the same order as they appeared in the original data and there are multiple data releases from this data set then potentially leaving the tuples in the order that they were in the original will allow someone to link information and even identify someone.

**Complementary Releases**: if multiple releases are made using various attributes from the same original data set someone could combine them to form a near complete original data set, breaking k-anonymity. Multiple releases from the same source should consider each other's attribute set member's of it's own quasi-identifier set when performing k-anonymity.

**Temporal Analysis**: When the original data is dynamic, multiple releases over time can be used to link back and isolate individuals. If an entry appears in one release but not another, they've become isolated to less than k-1. New releases need to look at previous releases and consider their information.

**Homogeneity Attacks**: If the various identifiers are k-anonymized but the "sensitive attributes" (not an identifier, usually something being studied like an illness that the person had, or something less depressing) have no or little variance then someone can learn something about a person without having to uniquely identify them.

**Outside Knowledge**: Some external dataset unrelated to this one, could provide me with data or statistics that allow me to narrow down to a smaller group. (This is also referred to as background knowledge but that makes it sound like you know something about the specific person that you're looking for and both this and the Homogeneity attacks kind of assume that because you're looking for a specific person)


# Differential Privacy
> what can be learned about any individual from a differentially private computation is (essentially) limited to what could be learned about them from everyone else’s data without their data being included in the computation.

Differential privacy comes from the work of Dwork [[4]], and takes the idea that someone's personal information should be as secure within the dataset as it is outside of it, and puts it into mathematical terms.

For instance if in one universe someone takes a survey and another the same someone opts-out of the survey. No one should be able to learn anything about that person from one dataset that they couldn't learn from the other. This has a caveat, if both of these datasets existed in the real world you could identify someone by comparing the two sets and seeing which entry is missing from one of the surveys. Differential Privacy should be able to protect against this as well.

As an example if 10000 people in the UK did a survey and from that it was found that 88% of white business men aged 40-50 dress in women's underwear and I know someone in the UK who is a 45 year old white business man, then I learned it's pretty likely that they dress in women's underwear (not that there is anything wrong with that, but it is information about them that they didn't consent to giving me and that I did not know before). However, in this case their privacy was the same whether they were in the study or not. I was able to learn about them simply because the study existed and differential privacy allows this. Studies can be done, and thus things can be learned about people, but I'm not learning this because of their choice about being in the study. Their decision to be in the study or not did not change their level of privacy provided.

Differential privacy doesn't protect against the statistical likeliness that you as a member of specific population group might be interested or more susceptible to certain ads, that you are likely to own a specific car, or have a particular political leaning. These things can all be statistically found without you being a part of the data set that came up with the statistic.

The carries into the idea that the results of any analysis should not depend on a single individual's response and remain the same whether a single individual takes part or not. Differential privacy requires only that the output of the analysis remain approximately the same between the opt-in and out scenario.

To implement differential privacy protection for individual data subjects, carefully-tuned random noise is added when producing statistics.

This allows more specific information to be revealed without risking the privacy of the individual. The reason for this is say you have a dataset table with survey responses. If you add noise to the answers, that still ensures that any analysis remains reasonably accurate, then even if someone identified who one of the survey responders is they wouldn't be completely sure that the responses are accurate. This makes the information unreliable, gives the individual deniability, and allows their privacy to remain intact.

A privacy parameter controls exactly how much information can be leaked and, correspondingly, how much random noise is added during the computation. It can be thought of as a tuning knob for balancing privacy and accuracy. You can make it more accurate and less private, or more private and less accurate. This is referred to as the privacy parameter &epsilon; which is the acceptable error, or difference between the real-world and opt-out scenario.

Since noise has been added to the outcome if the same analysis is performed twice on the dataset the results may be different. If implemented correctly the noise shouldn't alter the result greatly but intentionally adding random noise will change the results in this manner. It is possible to calculate accuracy bounds for the analysis that tell us how much the output of the analysis is expected to differ from the noiseless answer, allowing researchers to accommodate this error in their findings.

When &epsilon; is set to zero, the real-world differentially private analysis mimics the opt-out scenario of each individual perfectly. However, as we argued at the beginning of this section, an analysis that perfectly mimics the opt-out scenario of each individual would require ignoring all information from the input and accordingly could not provide any meaningful output. Yet when &epsilon; is set to a small number such as 0.1, the deviation between the real-world computation and each individual’s opt-out scenario will be small, providing strong privacy protection while also enabling an analyst to derive useful statistics based on the data.

Guidelines for choosing &epsilon; have not yet been developed   
As a rule of thumb, however, &epsilon; should be thought of as a small number, between approximately 1/1000 and 1.


[1]:https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3607247/
[2]:https://dataprivacylab.org/dataprivacy/projects/kanonymity/paper3.pdf   
[3]:https://dl.acm.org/doi/10.1142/S0218488502001648
[4]:https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/dwork.pdf
