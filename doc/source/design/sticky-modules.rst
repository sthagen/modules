.. _sticky-modules:

Sticky modules
==============

Configuration
-------------

- No specific configuration

Specification
-------------

- Once loaded a sticky module cannot be unloaded unless forced or reloaded.
- If module is super-sticky instead of sticky, it cannot be unloaded even if forced, only if it is reloaded afterward

- Stickyness definition relies on :ref:`module-tags`

    - ``module-tag sticky foo/1.0`` defines module *foo/1.0* as sticky
    - ``module-tag super-sticky foo/1.0`` defines module *foo/1.0* as super-sticky

- Stickyness specified over a symbolic version or a module alias has no effect

    - ``module-tag`` allows to specify a symbolic module version or a module alias
    - but associated tag will apply to the symbolic version or alias only
    - as modulefile targetted by symbol or alias does not inherit their tags
    - so a sticky or super-sticky tag set on a symbolic version or alias has no effect

- Sticky module can be swapped with another version from same module when stickness is defined over module parent name

    - For instance if stickyness is defined over module *foo*, *foo/1.0* can be swapped with *foo/2.0*
    - Such swap could occur from a ``restore`` or a ``switch`` sub-command
    - As soon as stickyness is defined over a precise module version name (like *foo/1.0*) such module cannot be swapped by another version of *foo* module
    - Stickyness defined over module parent name (like *foo*) means *any version from module foo must be loaded*
    - When stickyness is defined for several module versions using advanced version specifiers like *foo@1:* or *foo@1.0,2.0*

        - it means stickyness applies to the module versions
        - thus they cannot be swapped by another version

    - In case stickness is defined over module parent name and another ``module-tag`` defines stickyness over specific module version name

        - it means stickyness applies to the module version
        - thus these versions targetted specifically with ``module-tag`` cannot be swapped by another version from same module

- When a super-sticky module depends on a non-super-sticky module

    - If a forced ``purge`` command occurs, the dependent module will be unloaded
    - Which let the super-sticky module with a missing dependency

Current limitations
-------------------

- When swapping a sticky module explicitely targetted by the ``module-tag`` command and which is the default version

    - For instance ``module-tag sticky foo/1.0`` and ``module-version foo/1.0 default``
    - If specified swapped-on module is the generic module name, for instance *foo*
    - ``switch`` sub-command raises an error even if the sticky module is the default version (either implicit or explicitely set) for this module
