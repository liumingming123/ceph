.. _telemetry:

Telemetry Module
================

The telemetry module sends anonymous data about the cluster back to the Ceph
developers to help understand how Ceph is used and what problems users may
be experiencing.

Channels
--------

The telemetry report is broken down into several "channels," each with
a different type of information.  Assuming telemetry has been enabled,
individual channels can be turned on and off.  (If telemetry is off,
the per-channel setting has no effect.)

* **basic** (default: on): Basic information about the cluster

    - capacity of the cluster
    - number of monitors, managers, OSDs, MDSs, radosgws, or other daemons
    - software version currently being used
    - number and types of RADOS pools and CephFS file systems
    - names of configuration options that have been changed from their
      default (but *not* their values)

* **crash** (default: on): Information about daemon crashes, including

    - type of daemon
    - version of the daemon
    - operating system (OS distribution, kernel version)
    - stack trace identifying where in the Ceph code the crash occurred

* **device** (default: on): Information about device metrics, including

    - anonymized SMART metrics

* **ident** (default: off): User-provided identifying information about
  the cluster

    - cluster description
    - contact email address

The data being reported does *not* contain any sensitive
data like pool names, object names, object contents, hostnames, or device
serial numbers.

It contains counters and statistics on how the cluster has been
deployed, the version of Ceph, the distribution of the hosts and other
parameters which help the project to gain a better understanding of
the way Ceph is used.

Data is sent over HTTPS to *telemetry.ceph.com*.

Sample report
-------------

You can look at what data is reported at any time with the command::

  ceph telemetry show

To protect your privacy, device reports are generated separately, and data such
as hostname and device serial number is anonymized. The device telemetry is
sent to a different endpoint and does not associate the device data with a
particular cluster. To see a preview of the device report use the command::

  ceph telemetry show-device

Please note: In order to generate the device report we use Smartmontools
version 7.0 and up, which supports JSON output. 
If you have any concerns about privacy with regard to the information included in
this report, please contact the Ceph developers.

Channels
--------

Individual channels can be enabled or disabled with::

  ceph config set mgr mgr/telemetry/channel_ident false
  ceph config set mgr mgr/telemetry/channel_basic false
  ceph config set mgr mgr/telemetry/channel_crash false
  ceph config set mgr mgr/telemetry/channel_device false
  ceph telemetry show
  ceph telemetry show-device

Enabling Telemetry
------------------

To allow the *telemetry* module to start sharing data::

  ceph telemetry on

Please note: Telemetry data is licensed under the Community Data License
Agreement - Sharing - Version 1.0 (https://cdla.io/sharing-1-0/). Hence,
telemetry module can be enabled only after you add '--license sharing-1-0' to
the 'ceph telemetry on' command.

Telemetry can be disabled at any time with::

  ceph telemetry off

Interval
--------

The module compiles and sends a new report every 24 hours by default.
You can adjust this interval with::

  ceph config set mgr mgr/telemetry/interval 72    # report every three days

Status
--------

The see the current configuration::

  ceph telemetry status

Contact and Description
-----------------------

A contact and description can be added to the report.  This is
completely optional, and disabled by default.::

  ceph config set mgr mgr/telemetry/contact 'John Doe <john.doe@example.com>'
  ceph config set mgr mgr/telemetry/description 'My first Ceph cluster'
  ceph config set mgr mgr/telemetry/channel_ident true

