dispel4py
=========

ProvenanceType
--------------

.. code:: python

    ProvenanceType(self)

A workflow is a program that combines atomic and independent processing
elements via a specification language and a library of components. More
advanced systems adopt abstractions to facilitate re-use of workflows
across users' contexts and application domains. While methods can be
multi-disciplinary, provenance should be meaningful to the domain
adopting them. Therefore, a portable specification of a workflow
requires mechanisms allowing the contextualisation of the provenance
produced. For instance, users may want to extract domain-metadata from a
component or groups of components adopting vocabularies that match their
domain and current research, tuning the level of granularity. To allow
this level of flexibility, we explore an approach that considers a
workflow component described by a class, according to the
Object-Oriented paradigm. The class defines the behaviour of its
instances as their type, which specifies what an instance will do in
terms of a set of methods. We introduce the concept of *ProvenanceType*,
that augments the basic behaviour by extending the class native type, so
that a subset of those methods perform the additional actions needed to
deliver provenance data. Some of these are being used by some of the
preexisting methods, and characterise the behaviour of the specific
provenance type, some others can be used by the developer to easily
control precision and granularity. This approach, tries to balance
between automation, transparency and explicit intervention of the
developer of a data-intensive tool, who can tune provenance-awareness
through easy-to-use extensions.

The type-based approach to provenance collection provides a generic
*ProvenanceType* class that defines the properties of a provenance-aware
workflow component. It provides a wrapper that meets the provenance
requirements, while leaving the computational behaviour of the component
unchanged. Types may be developed as **Pattern Type** and **Contextual
Type** to represent respectively complex computational patterns and to
capture specific metadata contextualisations associated to the produce
output data.

The *ProvenanceType* presents the following class constants to indicate
where the lineage information will be stored. Options include a remote
repository, a local file system or a *ProvenanceSensor* (experimental).

-  *SAVE*\ MODE\_SERVICE='service'\_
-  *SAVE*\ MODE\_FILE='file'\_
-  *SAVE*\ MODE\_SENSOR='sensor'\_

The following variables will be used to configure some general
provenance capturing properties

-  *PROV*\ PATH\_: When *SAVE*\ MODE\_SERVICE\_ is chosen, this variable
   should be populated with a string indcating a file system path wher
   the lineage will be stored
-  *REPOS*\ URL\_: When *SAVE*\ MODE\_SERVICE\_ is chosen, this variable
   should be populated with a string indcating the repository endpoint
   (S-ProvFlow) where the provenance will be sent.
-  *PROV*\ DATA\_EXPORT\_URL: The service endpoint from where the
   provenance of a workflow execution, after being stored, can be
   extracted in PROV format.
-  *BULK*\ SIZE\_: Number of lineage documents to be stored in a single
   file or in a single request to the remote service. Helps tuning the
   overhead brough by the latency of accessing storage resources.

getProvStateObjectId
~~~~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.getProvStateObjectId(self, name)

Check if a data object with lookupterm *name*, is part of the provenance
state (*s-prov:StateCollection*) and returns its *id*.

makeProcessId
~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.makeProcessId(self)

Return the *id* to be attributed to an running instance
(*s-prov:ComponentInstance*) of a processing element.

makeUniqueId
~~~~~~~~~~~~

.. code:: python

    ProvenanceType.makeUniqueId(self, data, output_port)

In support of the implementation of a *ProvenanceType* realising a
lineage **Contextualisation type**. Return the *id* to be attributed to
a data entity (*s-prov:Data*) produced in output.

apply\_derivation\_rule
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.apply_derivation_rule(self, event, voidInvocation, oport=None, iport=None, data=None, metadata=None)

In support of the implementation of a *ProvenanceType* realising a
lineage *Pattern type*. This method is invoked by the *ProvenanceType*
each iteration when a decision has to be made whether to ignore or
discard the dependencies on the ingested stream and stateful entities,
applying a specific provenance pattern, thereby creating input/output
derivations. The framework invokes this method every time the data is
written on an output port (*event*: *write*) and every time an
invocation (*s-prov:Invocation*) ends (*event*:
*end*\ invocation\_event\_). The latter can be further described by the
boolean parameter *voidInvocation*, indicating whether the invocation
terminated with any data produced. The default implementation provides a
*stateless* behaviour, where the output depends only from the input data
recieved during the invocation.

getInputAt
~~~~~~~~~~

.. code:: python

    ProvenanceType.getInputAt(self, port='input', gindex=None)

Return input data currently available at a specific *port*. When reading
input of a grouped operator, the *gindex* parameter allows to access
exclusively the data related to the group index.

addNamespacePrefix
~~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.addNamespacePrefix(self, prefix, url)

In support of the implementation of a *ProvenanceType* realising a
lineage *Contextualisation type*. A Namespace *prefix* can be declared
with its vocabulary *url* to map the metadata terms to external
controlled vocabularies. They can be used to qualify the metadata terms
extracted from the *extractItemMetadata* function, as well as for those
terms injected selectively at runtime by the *write* method. The
namespaces will be used consistently when exporting the lineage traces
to semantic-web formats, such as RDF.

extractItemMetadata
~~~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.extractItemMetadata(self, data, port)

In support of the implementation of a *ProvenanceType* realising a
lineage *Contextualisation type*. Extracts metadata from the domain
specific content of the data (s-prov:DataGranules) written on a
components output *port*, according to a particular vocabulary.

ignorePastFlow
~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.ignorePastFlow(self)

In support of the implementation of a *ProvenanceType* realising a
lineage **Pattern type**.

It instructs the type to ignore the all the inputs when the method
*apply*\ derivation\_rule\_ is invoked for a certain event."

ignoreState
~~~~~~~~~~~

.. code:: python

    ProvenanceType.ignoreState(self)

In support of the implementation of a *ProvenanceType* realising a
lineage **Pattern type**.

It instructs the type to ignore the content of the provenance state when
the method *apply*\ derivation\_rule\_ is invoked for a certain event."

discardState
~~~~~~~~~~~~

.. code:: python

    ProvenanceType.discardState(self)

In support of the implementation of a *ProvenanceType* realising a
lineage **Pattern type**.

It instructs the type to reset the data dependencies in the provenance
state when the method *apply*\ derivation\_rule\_ is invoked for a
certain event. These will not be availabe in the following invocations."

discardInFlow
~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.discardInFlow(self, wlength=None, discardState=False)

In support of the implementation of a *ProvenanceType* realising a
lineage **Pattern type**.

It instructs the type to reset the data dependencies related to the
component''s inputs when the method *apply*\ derivation\_rule\_ is
invoked for a certain event. These will not be availabe in the following
invocations."

update\_prov\_state
~~~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.update_prov_state(self, lookupterm, data, location='', format='', metadata={}, ignore_inputs=False, ignore_state=True, **kwargs)

In support of the implementation of a *ProvenanceType* realising a
lineage *Pattern type* or inn those circumstances where developers
require to explicitly manage the provenance information within the
component''s logic,.

Updates the provenance state (*s-prov:StateCollection*) with a
reference, identified by a *lookupterm*, to a new *data* entity or to
the current input. The *lookupterm* will allow developers to refer to
the entity when this is used to derive new data. Developers can specify
additional *medatata* by passing a metadata dictionary. This will enrich
the one generated by the *extractItemMetadata* method. Optionally the
can also specify *format* and *location* of the output when this is a
concrete resource (file, db entry, online url), as well as instructing
the provenance generation to 'ignore\_input' and 'ignore\_state'
dependencies.

The *kwargs* parameter allows to pass an argument *dep* where developers
can specify a list of data *id* to explicitly declare dependencies with
any data in the provenance state (*s-prov:StateCollection*).

write
~~~~~

.. code:: python

    ProvenanceType.write(self, name, data, **kwargs)

This is the native write operation of dispel4py triggering the transfer
of data between adjacent components of a workflow. It is extended by the
*ProvenanceType* with explicit provenance controls through the *kwargs*
parameter. We assume these to be ignored when provenance is deactivated.
Also this method can use the lookup tags to establish dependencies of
output data on entities in the provenance state.

The *kwargs* parameter allows to pass the following arguments: - *dep* :
developers can specify a list of data *id* to explicitly declare
dependencies with any data in the provenance state
(*s-prov:StateCollection*). - *metadata*: developers can specify
additional medatata by passing a metadata dictionary. -
*ignore*\ inputs\_: instructs the provenance generation to ignore the
dependencies on the current inputs. - *format*: the format of the
output. - *location*: location of the output when this is a concrete
resource (file, db entry, online url).

checkSelectiveRule
~~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.checkSelectiveRule(self, streammeta)

In alignement with what was previously specified in the
configure\_prov\_run for the Processing Element, check the data granule
metadata whether its properies values fall in a selective provenance
generation rule.

checkTransferRule
~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.checkTransferRule(self, streammeta)

In alignement with what was previously specified in the
configure\_prov\_run for the Processing Element, check the data granule
metadata whether its properies values fall in a selective data transfer
rule.

extractDataSourceId
~~~~~~~~~~~~~~~~~~~

.. code:: python

    ProvenanceType.extractDataSourceId(self, data, port)

In support of the implementation of a *ProvenanceType* realising a
lineage *Pattern type*. Extract the id from the incoming data, if
applicable, to reuse it to identify the correspondent provenance entity.
This functionality is handy especially when a workflow component ingests
data represented by self-contained and structured file formats. For
instance, the NetCDF attributes Convention includes in its internal
metadata an id that can be reused to ensure the linkage and therefore
the consistent continuation of provenance tracesbetween workflow
executions that generate and use the same data.

AccumulateFlow
--------------

.. code:: python

    AccumulateFlow(self)

A *Pattern type* for a Processing Element (*s-prov:Component*) whose
output depends on a sequence of input data; e.g. computation of periodic
average.

Nby1Flow
--------

.. code:: python

    Nby1Flow(self)

A *Pattern type* for a Processing Element (*s-prov:Component*) whose
output depends on the data received on all its input ports in lock-step;
e.g. combined analysis of multiple variables.

SlideFlow
---------

.. code:: python

    SlideFlow(self)

A *Pattern type* for a Processing Element (*s-prov:Component*) whose
output depends on computations over sliding windows; e.g. computation of
rolling sums.

ASTGrouped
----------

.. code:: python

    ASTGrouped(self)

A *Pattern type* for a Processing Element (*s-prov:Component*) that
manages a stateful operator with grouping rules; e.g. a component that
produces a correlation matrix with the incoming coefficients associated
with the same sampling-iteration index

SingleInvocationFlow
--------------------

.. code:: python

    SingleInvocationFlow(self)

A *Pattern type* for a Processing Element (*s-prov:Component*) that
presents stateless input output dependencies; e.g. the Processing
Element of a simple I/O pipeline.

AccumulateStateTrace
--------------------

.. code:: python

    AccumulateStateTrace(self)

A *Pattern type* for a Processing Element (*s-prov:Component*) that
keeps track of the updates on intermediate results written to the output
after a sequence of inputs; e.g. traceable approximation of frequency
counts or of periodic averages.

IntermediateStatefulOut
-----------------------

.. code:: python

    IntermediateStatefulOut(self)

A *Pattern type* for a Processing Element (*s-prov:Component*) stateful
component which produces distinct but interdependent output; e.g.
detection of events over periodic observations or any component that
reuses the data just written to generate a new product

ForceStateless
--------------

.. code:: python

    ForceStateless(self)

A *Pattern type* for a Processing Element (*s-prov:Component*). It
considers the outputs of the component dependent only on the current
input data, regardless from any explicit state update; e.g. the user
wants to reduce the amount of lineage produced by a component that
presents inline calls to the *update*\ prov\_state\_, accepting less
accuracy.

get\_source
-----------

.. code:: python

    get_source(object, spacing=10, collapse=1)

Print methods and doc strings.

Takes module, class, list, dictionary, or string. ##
configure\_prov\_run

.. code:: python

    configure_prov_run(graph, provRecorderClass=None, provImpClass=<class 'dispel4py.provenance_doc.ProvenanceType'>, input=None, username=None, workflowId=None, description=None, system_id=None, workflowName=None, workflowType=None, w3c_prov=False, runId=None, componentsType=None, clustersRecorders={}, feedbackPEs=[], save_mode='file', sel_rules={}, transfer_rules={}, update=False)

To enable the user of a data-intensive application to configure the
attribution of types, selectivity controls and activation of advanced
exploitation mechanisms, we introduce the concept of provenance
configuration. With the configuration users can specify a number of
properties, such as attribution, provenance types, clusters, sensors,
selectivity rules, etc. The configuration is used at the time of the
initialisation of the workflow to prepare its provenance-aware
execution. We consider that a chosen configuration may be influenced by
personal and community preferences, as well as by rules introduced by
institutional policies. For instance, a Research Infrastructure (RI) may
indicate best practices to reproduce and describe the operations
performed by the users exploiting its facilities, or even impose
requirements which may turn into quality assessment metrics. This could
require to choose among a set of contextualisation types, in order to
adhere to the infrastructure's metadata portfolio. Thus, a provenance
configuration profile play in favour of more generality, encouraging the
implementation and the re-use of fundamental methods across disciplines.

With this method, the users of the workflow provide general provenance
information on the attribution of the run, such as *username*, *runId*
(execution id), *description*, *workflowName*, and its semantic
characterisation *workflowType*. It allows users to indicate which
provenance types to apply to each component and the belonging conceptual
provenance cluster. Moreover, users can also choose where to store the
lineage (*save*\ mode\_), locally in the file system or in a remote
service or database. Lineage storage operations can be performed in
bulk, with different impacts on the overall overhead and on the
experienced rapidity of access to the lineage information.

-  **Selectivity and Transfer rules**: By declaratively indicating a set
   of Selectivity and Transfer rules for every component
   (*sel*\ rules\_, *transfer*\ rules\_), users can respectively
   activate the collection of the provenance for particular Data
   elements or trigger transfer operations of the data to external
   locations. The approach takes advantage of the contextualisation
   possibilities offered by the provenance *Contextualisation types*.
   The rules consist of comparison expressions formulated in JSON that
   indicate the boundary values for a specific metadata term. Such
   representation is inspired by the query language and selectors
   adopted by a popular document store, MongoDB.

Example, a Processing Element *CorrCoef* that produces lineage
information only when the *rho* value is greater than 0:

.. code:: python

        { "CorrCoef": {
            "rules": {
                "rho": {
                    "$gt": 0
        }}}}

ProvenanceSimpleFunctionPE
--------------------------

.. code:: python

    ProvenanceSimpleFunctionPE(self, *args, **kwargs)

A *Pattern type* for the native *SimpleFunctionPE* of dispel4py

ProvenanceIterativePE
---------------------

.. code:: python

    ProvenanceIterativePE(self, *args, **kwargs)

A *Pattern type* for the native *IterativePE* Element of dispel4py
