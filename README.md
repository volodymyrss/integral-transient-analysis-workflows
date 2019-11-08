# Restrictions

INTEGRAL data is subject to observing time proposals and is reserved for the observer PI for up to one year. The communications are mostly public.

# Event generation

We have developed a system around [INTEGRAL Burst Alert System](https://www.isdc.unige.ch/integral/science/grb), 
In addition, ISDC (INTEGRAL Science Data Center) handles the primary data reduction and produces some evaluation of the data, yielding for example new detected sources and reports of observations (the Quick Look Analysis).

The results are distributed in GCN notices as well as semi-automated and human-readable GCN circulars and ATels.
There is also experimental upload to [zenodo](https://sandbox.zenodo.org/search?page=1&size=20&q=keywords:%22S191105e%22)

So there are various kinds of events generated from INTEGRAL, some by very old systems, some by rather new, distributed by IBAS, GCN, even just HTTP POST. It does not really matter, since the event rate is not high (perhaps hundreds per day, and a subset useful for GRB is even smaller).

# Event consumption

In addition, we developed an system to react on external events/triggers. It's integrated in the ISDC infrastructure (such as the direct telemetry stream from the spacecraft),  and deals with otherwise sensitive data, subject to clear formal restrictions. It exists in the current form since this April, and used by the collaboration for follow-up of the multi-messenger transients.
Since its restricted there was no need so far to make a public document about it.
It's also because its primarily infrastructure, with combines existing software, and some custom software addressing specific clear goals (a lot of which is public, see below).
The system was presented at several conferences. 
Various CDCI-related projects associated to this system are hosted here:  https://github.com/cdcihub.

The system ingests events from various sources (hence similarly to the event production, the consumption needs to deal with diverse interfaces and there is no problem to add any new ones).  At lot of it is GCN notices, as well some NLP of GCN circulars and ATels. Experimental event reception is arranged with Apache Kafka.

The events are classified and treated by collection of diverse workflows of different kind. They are quite diverse indeed, and what unities them is they are executable in some universal way (which means that they are appropriately annotated, e.g. as an OpenAPI service, or as CWL description), and reproducible (i.e. they produce the same result for the same input, and hence can be cached). This allows to assign then unique identities, keep track of them in the rdf store (in Jena), keep track of meaningful provenance, etc.

Most of the transient analysis workflows live as [OpenAPI](https://github.com/OAI/OpenAPI-Specification) services, which makes them responsive and distributed (kept near the data).

The online data analysis (see [frontend](https://www.astro.unige.ch/cdci/astrooda_)) is also treated as one of such workflows. Internally, it fetches workflows from github.

Many of the these services  are build with gitlab CI from jupyther notebooks (with https://github.com/volodymyrss/nb2workflow), so that expert contributors can develop in their specific area of expertise and they get integrated automatically.
What is primarily seen by the users is the "dashboard"  which presents various selection of the results of the event treatment (and allows to request new ones).

The heterogeneous design is intentional, and natural for the multi-messenger domain, with multiple parties with different practices are interfacing. So a lot of the time we try on building adopters to the interfaces instead of forcing the same interface.

We focus on developing the scientific workflows in universal reproducible way, and develop interfaces to various external providers (e.g. we consume some services from [ESAC](https://www.esa.int/About_Us/ESAC)).
This also allows to substitute different designs of the infrastructure without changing the scientific data analysis workflows, only recombining them differently: for faster realtime processing, with different scientific contexts, etc.

The products of this system also end up in the same channels described above: GCN notices, circulars, zenodo.
