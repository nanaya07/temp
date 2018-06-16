Vehicle to vehicle scenario
---------------------------

.. sidebar:: Topology

    .. aafig::
        :aspect: 60
        :scale: 90

          +----------+        +--------+        +--------+        +--------+        +----------+
          |    0     |        |   1    |        |   2    |        |   3    |        |    4     |
          | Producer |<------>| Router |<------>| Router |<------>| Router |<------>| Consumer |
          |          |        |        |        |        |        |        |        |          |
          +----------+        +--------+        +--------+        +--------+        +----------+
          
          There is only wifi face
          All nodes can move

The vehicle to vehicle example (``ndn-v2v-moving.cpp``) shows basics of mobile ad-hoc on ndnSIM.
This scenario simulates a scenario with 5 vehicles moving and communicating.
Its simulation time is 20 seconds.
There are one NDN consumer (node 4), one NDN producer (node 0), and 3 Ad-hoc routers:

Consumer is simulated using :ndnsim:`ConsumerCbr` reference application and generates Interests
towards the producer with frequency of 1 Interest per second (see :doc:`applications`).

Producer is simulated using :ndnsim:`Producer` class, which is used to satisfy all incoming
Interests with virtual payload data (1024 bytes).

Router is simulated using :ndnsim:`Consumer-v2v` class, which can send back both incoming Interests
and incoming data.

There are 4 moving node options in this scenario:

option 0, All node stays in same positon.

option 1 (default), All node moves with same speed.

option 2, All node moves with same speed initially. 
Then, they change their direction oppositely when it gets half of simulation time.

option 3, All node moves with different speed. The range of movement in 50 from origin positon

The movement option can be chosen following variable in line 80 of the scenario::

     int test = 1;

The following code represents all that is necessary to run such a
Vehicle to vehicle scenario

.. literalinclude:: ../../examples/ndn-v2v-moving.cpp
   :language: c++
   :linenos:
   :lines: 1-22,56-73,80-
   :emphasize-lines: 80-138

In order to run scenario and see what is happening, you can use the following command::

      NS_LOG=ndn.Consumer:ndn.Producer:ndn.Consumerv2v ./waf --run=ndn-v2v-moving

If you want to see how nodes move, you can use the following command::

     NS_LOG=V2VMoving ./waf --run=ndn-v2v-moving

.. note::
   Before run the scenario, it requires to enable Ad-hoc mode.

   Currently, it can be done with commenting out following code in ``/NFD/deamon/fw/Forwarder.cpp``
   under the method: onIncomingData(Face& inFace, const Data& data)::

         if (pendingDownstream->getId() == inFace.getId() &&
         pendingDownstream->getLinkType() != ndn::nfd::LINK_TYPE_AD_HOC) {
         continue;
         }
