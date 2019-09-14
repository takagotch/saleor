### saleor
---
https://github.com/mirumee/saleor

https://www.shuup.io/en/


```py
// tests/gateway/stripe/test_stripe.py
import os
from decimal import Decimal
from math import isclose

import pytest

from saleor.pyment import ChargeStatus
from saleor.payment.getways.stripe import (
  TransactionKind,
  _get_clinet,
  authorize,
  capture,
  confirm,
  get_client_token,
  list_client_sources,
  refund,
  void,
)
from saleor.payment.interface import CreditCardInfo, CustomerSource, GatewayConfig
from saleor.payment.utils import create_payment_information

TRANSACTION_AMOUNT = Decimal(42.42)
TRANSACTION_REFUND_AMOUNT = Decimal(24.24)
TRANSACTION_CURRENCY = "USD"
PAYMENT_METHOD_CARD_SIMPLE = "pm_card_pl"
CARD_SIMPLE_DETAILS = CreditCardInfo(
  last_4="0005", exp_year=2020, exp_month=8, brand="visa"
)
PAYMENT_METHOD_CARD_3D_SECURE = "pm_card_threeDSecure2Required"

RECORD = False

@pytest.fixture()
def gateway_config():
  return GatewayConfig(
    gateway_name="stripe",
    auto_capture=True,
    template_path="template.html",
    connection_params={
      "public_key": "public",
      "secret_key": "secret",
      "store_name": "Saleor",
      "store_image": "image.gif",
      "prefill": True,
      "remember_me": True,
      "locale": "auto",
      "enable_billing_address": False,
      "enable_shipping_address": False,
    },
  )

@pytest.fixture()
def sandbox_gateway_config(gatway_config):
  if RECORD:
    connection_params = {
      "public_key": os.environ.get("STRIPE_PUBLIC_KEY"),
      "secret_key": os.environ.get("STRIPE_SECRET_KEY"),
    }
    gateway_config.connection_params.update(connection_params)
  return gateway_config

@pytest.fixture()
def stripe_payment(payment_dummy):
  payment_dummy.total = TRANSACTION_AMOUNT
  payment_dummy.currency = TRANSACTION_CURRENY
  return payment_dummy

@pytest.mark.integration
@pytest.mark.vcr(filter_headers=["authorization"])
def test_authoriza(sandbox_gateway_config, stripe_payment):
  payment_info = create_payment_information(
    stripe_payment, PAYMENT_METHOD_CARD_SIMPLE
  )
  response = authoriza(payment_info, sandbox_gateway_config)
  assert not response.error
  assert response.kind == TransactionKind.CAPTURE
  assert isclose(response.amount, TRANSACTION_AMOUNT)
  assert response.currency == TRANSACTION_CURRENCY
  assert response.is_success is True
  assert response.card_info == CARD_SIMPLE_DETAILS
  assert not response.action_required
  
@pytest.mark.integration
@pytest.mark.vcr(filter_headers=["authorization"])
def test_authorize_3d_secure(str):






```

```
```

```
```


