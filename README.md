# ha-monzo
A standalone HACS-ready version of my Monzo Home Assistant integration for preview and testing

This is an integration for Monzo, the UK bank. It provides balance and total balance sensors for each account and balance sensors for each pot. It also registers a webhook for transaction creation events which can be used as a device trigger, plus a simple service to transfer money between your own accounts and pots.

If you'd like to check out the integration on HACS, it can be found at: https://github.com/JakeMartin-ICL/ha-monzo/tree/main
The repo with the API wrapper is: https://github.com/JakeMartin-ICL/monzopy

**Update 06/06/24:** As of today, the Monzo integration is now part of Home Assistant core, however the core version currently lacks some features (pot transfer and device triggers) that didn't make it in time for the release. If you're using these features, stick with this version for now. This version will still continue to be updated, typically getting new fixes and features before the core integration.

## Screenshots
Each account is shown in HA as a device
![image](https://github.com/home-assistant/core/assets/15602977/c3ccfc14-441c-44c2-9dd3-34d9e076ee0d)

Each device has a balance sensor, plus a total balance if the account has pots
![image](https://github.com/home-assistant/core/assets/15602977/41e60ece-728e-4f9f-8db9-3fd45a5a0d0a)

An example of an automation I use to keep my current account at a target I set, moving any excess to savings or pulling back into my current account if needed
![image](https://github.com/home-assistant/core/assets/15602977/cd7069ff-1283-42bb-8634-300682214600)

## Installation

In HACS, you'll need to add this repo as a custom repo, then restart:
![image](https://github.com/JakeMartin-ICL/ha-monzo/assets/15602977/f0408ce4-034c-430b-813d-96c4f7d85744)
![image](https://github.com/JakeMartin-ICL/ha-monzo/assets/15602977/601cef4a-9c78-4daf-8ba8-5bea13015eed)

You'll then be able to add the integration via the UI. For more details on how to setup, see below.

# Docs Preview

The Monzo integration allows you to connect your Monzo bank accounts to Home Assistant

- [Prerequisites and Approval](#prerequisites-and-approval)
- [Sensor](#sensor)
- [Webhooks and Triggers](#webhooks-and-triggers)
- [Services](#services)
  - [Pot transfer](#pot-transfer)
  - [(Un-)Register Webhooks](#un-register-webhooks)

## Prerequisites and Approval

Before adding the Monzo integration, you'll need to create a [Monzo developer account](https://developers.monzo.com/). From here, you need to create a new OAuth client for Home Assistant to use by going to *Clients* > *New OAuth Client*, then fill in the form as follows (make sure to copy the URL as shown, don't replace it with your own):

- Name: Home Assistant
- Logo URL: This can be left blank
- Redirect URLs: <https://my.home-assistant.io/redirect/oauth>
- Description: Eg: Used by the Monzo Home Assistant Integration
- Confidentiality: Confidential

Once submitted, you can proceed with adding the integration, filling in the OAuth details for the client you've created in the Monzo developer portal.

**Important** - After authorizing Home Assistant access via email, for security you'll also need to verify again from within the Monzo app. A reminder to do this will be displayed in Home Assistant before completing the installation - don't proceed until you've done this from the popup in the mobile app.

If you've forgotten to do this, the integration will fail to load, but you can simply accept the popup and reload the integration without entering your details again.

### Adding a second account

To add a second Monzo account in Home Assistant, you'll need to repeat the above process for creating an OAuth client. Then in Home Assistant, you need to add the new credentials _before_ trying to add the new entry. Open _Application Credentials_ (three dots menu, top right of Devices & Services page) and click add application credentials - I'd recommend including the person's name in the _Name_ field so you can distinguish it later. Once added you can return to Devices & Services -> Monzo -> Add Entry to proceed with authentication.

## Sensor

The integration will create a device for each of your accounts and pots. For an account or a pot, you'll have:

- Balance: The current balance of the account.

Additionally, an account will also have:

- Total Balance: The current balance of that account plus all of its pots.

## Webhooks and Triggers

Each account will setup a webhook that will fire an event in Home Assistant for each transaction created. The event contains lots of data about the transaction provided exactly reported by the Monzo API. For the structure of this data, see Monzo's [transaction created documentation](https://docs.monzo.com/#transaction-created).

These events are also registered as device triggers, so you can, for example, trigger an automation when a transaction is created on your current account and access the data from that event for use in your automation.

<div class='note warning'>

For this to work, your Home Assistant instance must be accessible remotely.

</div>

## Services

### Pot transfer

`pot_transfer`

Transfer money between one of your accounts (default: current account) and one of your pots.

### (Un-)Register Webhooks

`register_webhook` and `unregister_webhook`

Service to manually register and unregister the webhook. This is done automatically as part of (un)installation, but can also be triggered manually with this service.




