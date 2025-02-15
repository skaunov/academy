===========
Get Started
===========

.. title::
   BasicSwap DEX Installation Guide
   
.. meta::
   :description lang=en: Learn how to install BasicSwap DEX on your desktop computer 
   :keywords lang=en: Particl, DEX, Trading, Exchange, Buy Crypto, Sell Crypto, Installation, Quickstart, Blockchain, Privacy, E-Commerce, multi-vendor marketplace, online marketplace

:term:`BasicSwap DEX <BasicSwap>` is a cross-chain and privacy-centric DEX (decentralized exchange) that lets you trade cryptocurrencies without middleman interference. This section walk you through all the steps required to install it and set it up to your liking.

----

.. contents:: Table of Contents
   :local:
   :backlinks: none
   :depth: 2

----

.. attention::

	 If you want to install BasicSwap using Tor, then jump to the installation section of the BasicSwap Tor Guide.

Install Using Docker
====================

Because :term:`BasicSwap` is still in early beta, there is no ready-made executable or convenient integrations (i.e., Particl Desktop, web gateway, third-party platforms, etc) yet. For this reason, the first step to get started is to compile the application locally on your computer.

Install Docker
--------------

The easiest way to setup :term:`BasicSwap` is through Docker. Note that this setup method is not available for OSX, so if you've got a Mac device, please refer to the next section.

.. tabs::

     .. group-tab:: Windows

     .. group-tab:: Linux

         **Install the Docker engine on your device**

         .. rst-class:: bignums

             #. Install the required dependencies.

                 .. code-block:: bash

                     apt-get install curl jq git

             #. Verify if you've already got Docker installed.

             	 .. code-block:: bash
             	 	 
             	 	 docker -v

             	 If already installed, you should see a line containing :guilabel:`Docker version (...)`. If that's the case, you don't need to re-install it and you can safely skip to the next section.

             #. Install Docker by following `the instructions on their website <https://docs.docker.com/engine/install/#server>`_.

             #. It is recommended to `setup Docker without sudo <https://docs.docker.com/engine/install/linux-postinstall/>`_. Otherwise, you'll be required to preface each :guilabel:`docker-compose` command with :guilabel:`sudo` every time.

Create the Docker Image
-----------------------

Create BasicSwap's docker image. You will need to run this image whenever you want to run the DEX.

.. tabs::

     .. group-tab:: Windows

     	 .. rst-class:: bignums

             #. Open a WSL (Linux) terminal.

                 :kbd:`Windows` + :kbd:`R` > "wsl" -> :kbd:`ENTER`.

             #. Install Git.

             	 .. code-block:: bash
             	 	 
             	 	 sudo apt update
             	 	 sudo apt install git

             #. Download the BasicSwap code.

             	 .. code-block:: bash

             	 	 git clone https://github.com/tecnovert/basicswap.git

             #. Navigate to BasicSwap's Docker folder.

                 .. code-block:: bash

             	 	 cd basicswap/docker/

             #. Set your :guilabel:`COINDATA_PATH`.

                 .. code-block:: bash

             	 	 export COINDATA_PATH=/var/data/coinswaps 

             #. Create the BasicSwap Docker image (make sure you are in :guilabel:`basicswap/docker`.

                     .. code-block:: bash
				 
             	 	 docker-compose build


     .. group-tab:: Linux

         .. rst-class:: bignums

                 #. Open a terminal.

                 #. Download the BasicSwap code.

                     .. code-block:: bash

	             	 	 git clone https://github.com/tecnovert/basicswap.git

                 #. Navigate to BasicSwap's Docker folder.

                     .. code-block:: bash

	             	 	 cd basicswap/docker/

                 #. Set your :guilabel:`COINDATA_PATH`.

                     .. code-block:: bash

	             	 	 export COINDATA_PATH=/var/data/coinswaps 

                 #. Create the BasicSwap Docker image (make sure you are in :guilabel:`basicswap/docker`.

                     .. code-block:: bash
					 
	             	 	 docker-compose build        	 	 


Configure the Docker Image
--------------------------

Now that BasicSwap's image has been created, you need to configure it to your liking. 

.. tabs::

     .. group-tab:: Windows

     	 .. rst-class:: bignums

             #. Open a WSL (Linux) terminal.

                 :kbd:`Windows` + :kbd:`R` > "wsl" -> :kbd:`ENTER`.

             #. Navigate to BasicSwap's Docker folder.

                 .. code-block:: bash

             	 	 cd basicswap/docker/

             #. Set :guilabel:`xmrrestoreheight` to Monero's current chain height.

             	 .. code-block:: bash

             	 	 CURRENT_XMR_HEIGHT=$(curl https://localmonero.co/blocks/api/get_stats | jq .height)

             #. Choose what coins you want to enable (Particl will be enabled by default). You will need to write them in the configuration command. :ref:`Click here <compatible coins>` for a full list of available coins on BasicSwap.

             #. Decide whether you want to fast sync the Bitcoin blockchain by downloading a checkpoint or sync from scratch. This will be determined by whether or not you add the :guilabel:`--usebtcfastsync` argument to the configuration command.

             #. Configure your BasicSwap Docker image by typing the following configuration command. Make sure to change it according to your preferences as mentioned in the previous two steps.

             	 .. code-block:: bash

             	 	 export COINDATA_PATH=/var/data/coinswaps && docker run --rm -t --name swap_prepare -v $COINDATA_PATH:/coindata i_swapclient basicswap-prepare --datadir=/coindata --withcoins=monero,bitcoin --htmlhost="0.0.0.0" --wshost="0.0.0.0" --xmrrestoreheight=$CURRENT_XMR_HEIGHT --usebtcfastsync

             #. Note the mnemonic that the previous command will give you somewhere safe. This is your wallet backup key.

             #. Note the output of the following command somewhere safe. It is useful if you need to restore your Monero wallet later on.

                 .. code-block:: bash

             	 	 echo $CURRENT_XMR_HEIGHT

             #. **(Optional)** Set your timezone by setting the correct :guilabel:`TZ` value in your :guilabel:`.env` file (located in BasicSwap's docker folder). List valid timezone options by typing the :guilabel:`timedatectl list-timezones` command.

             	 .. code-block:: bash
             	 
             	 	 nano .env

             	 To save changes, press :kbd:`CTRL` + :kbd:`X`, then :kbd:`Y` + :kbd:`ENTER`.


     .. group-tab:: Linux

     	 .. rst-class:: bignums

             #. Open a terminal.

             #. Navigate to BasicSwap's Docker folder.

                 .. code-block:: bash

             	 	 cd basicswap/docker/

             #. Set :guilabel:`xmrrestoreheight` to Monero's current chain height.

             	 .. code-block:: bash

             	 	 CURRENT_XMR_HEIGHT=$(curl https://localmonero.co/blocks/api/get_stats | jq .height)

             #. Choose what coins you want to enable (Particl will be enabled by default). You will need to write them in the configuration command. :ref:`Click here <compatible coins>` for a full list of available coins on BasicSwap.

             #. Decide whether you want to fast sync the Bitcoin blockchain by downloading a checkpoint or sync from scratch. This will be determined by whether or not you add the :guilabel:`--usebtcfastsync` argument to the configuration command.

             #. Configure your BasicSwap Docker image by typing the following configuration command. Make sure to change it according to your preferences as mentioned in the previous two steps.

             	 .. code-block:: bash

             	 	 export COINDATA_PATH=/var/data/coinswaps && docker run --rm -t --name swap_prepare -v $COINDATA_PATH:/coindata i_swapclient basicswap-prepare --datadir=/coindata --withcoins=monero,bitcoin --htmlhost="0.0.0.0" --wshost="0.0.0.0" --xmrrestoreheight=$CURRENT_XMR_HEIGHT --usebtcfastsync

             #. Note the mnemonic that the previous command will give you somewhere safe. This is your wallet backup key.

             #. Note the output of the following command somewhere safe. It is useful if you need to restore your Monero wallet later on.

                 .. code-block:: bash

             	 	 echo $CURRENT_XMR_HEIGHT

             #. **(Optional)** Set your timezone by setting the correct :guilabel:`TZ` value in your :guilabel:`.env` file (located in BasicSwap's docker folder). List valid timezone options by typing the :guilabel:`timedatectl list-timezones` command.

             	 .. code-block:: bash

             	 	 nano .env

             	 To save changes, press :kbd:`CTRL` + :kbd:`X`, then :kbd:`Y` + :kbd:`ENTER`.

Start BasicSwap
---------------

Now that you've configured your docker image, it's time to run it. This will start BasicSwap and make it accessible from web browsers.

.. tabs::

     .. group-tab:: Windows

     	 .. rst-class:: bignums

             #. Open a WSL (Linux) terminal.

                 :kbd:`Windows` + :kbd:`R` > "wsl" -> :kbd:`ENTER`.

             #. Navigate to BasicSwap's Docker folder.

                 .. code-block:: bash

             	 	 cd basicswap/docker/

             #. Start the Docker image. This will launch BasicSwap's startup process.

             	 .. code-block:: bash

             	 	 export COINDATA_PATH=/var/data/coinswaps && docker-compose up

             #. Wait for BasicSwap to start fully, then open it up in your favorite web browser by navigating to the following address.

                 .. code-block:: bash

             	 	 http://localhost:12700

     .. group-tab:: Linux
 
             .. rst-class:: bignums
 
                 #. Open a terminal.
 
                 #. Navigate to BasicSwap's Docker folder.
 
                     .. code-block:: bash
 
                         cd basicswap/docker/
 
                 #. Start the Docker image. This will launch BasicSwap's startup process.

                     .. code-block:: bash

                         export COINDATA_PATH=/var/data/coinswaps && docker-compose up

                 #. Wait for BasicSwap to start fully, then open it up in your favorite web browser by navigating to the following address.

                     .. code-block:: bash

                         http://localhost:12700

Install Without Docker
======================

.. tabs::

     .. group-tab:: Mac OS
 
             .. rst-class:: bignums
 
                 #. Open :guilabel:`Terminal` (i.e., :kbd:`COMMAND ⌘` + :kbd:`SPACE` and type "terminal" > hit :kbd:`ENTER ↵`).

                 #. Install Homebrew, which will let you execute Linux-like commands right from your Mac OS terminal.

                     .. code-block::

                         /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

                 #. Install the required dependencies

                     .. code-block::

                         brew install wget unzip python git protobuf gnupg automake libtool pkg-config curl jq

                 #. Close the terminal and open a new one. This will update the python symlinks and allow you to progress through the next steps.

                 #. Execute the following commands **one by one** (do NOT copy-paste the entire block at once). For each command entered, ensure that the terminal doesn't return any error. If it does, carefully look at what the error is and fix it before moving to the next step; entering the next command without fixing a previous issue will break the installation process.

                     .. code-block::

                         export SWAP_DATADIR=/Users/$USER/coinswaps
                         mkdir -p "$SWAP_DATADIR/venv"
                         python3 -m venv "$SWAP_DATADIR/venv"
                         . $SWAP_DATADIR/venv/bin/activate && python -V
                         cd $SWAP_DATADIR
                         wget -O coincurve-anonswap.zip https://github.com/tecnovert/coincurve/archive/refs/tags/anonswap_v0.1.zip
                         unzip -d coincurve-anonswap coincurve-anonswap.zip
                         mv ./coincurve-anonswap/*/{.,}* ./coincurve-anonswap || true
                         cd $SWAP_DATADIR/coincurve-anonswap
                         pip3 install .
                         cd $SWAP_DATADIR
                         git clone https://github.com/tecnovert/basicswap.git 
                         cd $SWAP_DATADIR/basicswap

                 #. Install root SSL certificates for the SSL module (more information `here <https://pypi.org/project/certifi/>`_.

                         .. code-block::

                             sudo python3 bin/install_certifi.py

                 #. Continue with the BasicSwap installation, executing the following two commands **one by one**.
                         
                         .. code-block::

                             protoc -I=basicswap --python_out=basicswap basicswap/messages.proto
                             pip3 install .

     .. group-tab:: Linux
 
             .. rst-class:: bignums
 
                 #. Install the required dependencies

                     .. code-block::

                         apt-get install -y wget python3-pip gnupg unzip protobuf-compiler automake libtool pkg-config curl jq

                 #. Execute the following commands **one by one** (do NOT copy-paste the entire block at once). For each command entered, ensure that the terminal doesn't return any error. If it does, carefully look at what the error is and fix it before moving to the next step; entering the next command without fixing a previous issue will break the installation process.

                     .. code-block::

                         export SWAP_DATADIR=/Users/$USER/coinswaps
                         mkdir -p "$SWAP_DATADIR/venv"
                         python3 -m venv "$SWAP_DATADIR/venv"
                         . $SWAP_DATADIR/venv/bin/activate && python -V
                         cd $SWAP_DATADIR
                         wget -O coincurve-anonswap.zip https://github.com/tecnovert/coincurve/archive/refs/tags/anonswap_v0.1.zip
                         unzip -d coincurve-anonswap coincurve-anonswap.zip
                         mv ./coincurve-anonswap/*/{.,}* ./coincurve-anonswap || true
                         cd $SWAP_DATADIR/coincurve-anonswap
                         pip3 install .
                         cd $SWAP_DATADIR
                         git clone https://github.com/tecnovert/basicswap.git 
                         cd $SWAP_DATADIR/basicswap
                         protoc -I=basicswap --python_out=basicswap basicswap/messages.proto
                         pip3 install .

Configure the Docker Image
--------------------------

.. tabs::

     .. group-tab:: Mac OS
 
             .. rst-class:: bignums

                 #. Open :guilabel:`Terminal` (i.e., :kbd:`COMMAND ⌘` + :kbd:`SPACE` and type "terminal" > hit :kbd:`ENTER ↵`).

                 #. Navigate to your BasicSwap folder.

                     .. code-block:: bash

                         cd /Users/$USER/coinswaps

                 #. Set :guilabel:`xmrrestoreheight` to Monero's current chain height.

                     .. code-block:: bash

                         CURRENT_XMR_HEIGHT=$(curl https://localmonero.co/blocks/api/get_stats | jq .height)

                 #. Choose what coins you want to enable (Particl will be enabled by default). You will need to write them in the configuration command. :ref:`Click here <compatible coins>` for a full list of available coins on BasicSwap.

                 #. Decide whether you want to fast sync the Bitcoin blockchain by downloading a checkpoint or sync from scratch. This will be determined by whether or not you add the :guilabel:`--usebtcfastsync` argument to the configuration command.

                 #. Configure your BasicSwap Docker image by typing the following configuration command. Make sure to change it according to your preferences as mentioned in the previous two steps.

                     .. code-block:: bash

                         basicswap-prepare --datadir=$SWAP_DATADIR --withcoins=monero,bitcoin --xmrrestoreheight=$CURRENT_XMR_HEIGHT --usebtcfastsync

     .. group-tab:: Linux
 
             .. rst-class:: bignums
 
                 #. Open a terminal.

                 #. Navigate to your BasicSwap folder.

                     .. code-block:: bash

                         cd /Users/$USER/coinswaps

                 #. Set :guilabel:`xmrrestoreheight` to Monero's current chain height.

                     .. code-block:: bash

                         CURRENT_XMR_HEIGHT=$(curl https://localmonero.co/blocks/api/get_stats | jq .height)

                 #. Choose what coins you want to enable (Particl will be enabled by default). You will need to write them in the configuration command. :ref:`Click here <compatible coins>` for a full list of available coins on BasicSwap.

                 #. Decide whether you want to fast sync the Bitcoin blockchain by downloading a checkpoint or sync from scratch. This will be determined by whether or not you add the :guilabel:`--usebtcfastsync` argument to the configuration command.

                 #. Configure your BasicSwap Docker image by typing the following configuration command. Make sure to change it according to your preferences as mentioned in the previous two steps.

                     .. code-block:: bash

                         basicswap-prepare --datadir=$SWAP_DATADIR --withcoins=monero,bitcoin --xmrrestoreheight=$CURRENT_XMR_HEIGHT --usebtcfastsync

Start BasicSwap
---------------

.. tabs::

     .. group-tab:: Mac OS
 
             .. rst-class:: bignums

                 #. Open :guilabel:`Terminal` (i.e., :kbd:`COMMAND ⌘` + :kbd:`SPACE` and type "terminal" > hit :kbd:`ENTER ↵`).

                 #. Navigate to your BasicSwap folder.

                     .. code-block:: bash

                         cd /Users/$USER/coinswaps

                 #. Launch BasicSwap.

                     .. code-block:: bash

                         basicswap-run --datadir=$SWAP_DATADIR

                 #. Open BasicSwap's user interface in your favorite web browser by navigating to the following address.

                     .. code-block:: bash

                         http://localhost:12700

     .. group-tab:: Linux
 
             .. rst-class:: bignums
 
                 #. Open a terminal.

                 #. Navigate to your BasicSwap folder.

                     .. code-block:: bash

                         cd /Users/$USER/coinswaps

                 #. Launch BasicSwap.

                     .. code-block:: bash

                         basicswap-run --datadir=$SWAP_DATADIR

                 #. Open BasicSwap's user interface in your favorite web browser by navigating to the following address.

                     .. code-block:: bash

                         http://localhost:12700