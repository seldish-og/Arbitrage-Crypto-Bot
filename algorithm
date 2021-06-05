from binance.client import Client

import new_crypto_dict
import rounds


class Config:
    API_KEY = 'KEY'

    SECRET_KEY = 'SecretKey'


class CreateDeal:
    def __init__(self):
        self.client = Client(Config.API_KEY, Config.SECRET_KEY)
        self.prices = self.client.get_all_tickers()

    def buy_market(self, symbol, quantity):
        o = self.client.create_order(
            symbol=symbol,
            side=Client.SIDE_BUY,
            type=Client.ORDER_TYPE_MARKET,
            quantity=quantity)

    def sell_market(self, symbol, quantity):
        o = self.client.create_order(
            symbol=symbol,
            side=Client.SIDE_SELL,
            type=Client.ORDER_TYPE_MARKET,
            quantity=quantity)

    # get_symbol_ticker('ETHUSDT') result: {'symbol': 'ETHUSDT', 'price': '3987.15000000'}
    # float(self.client.get_symbol_ticker(symbol=(x[lenght:] + 'USDT'))['price']) # getting price in USDT
    def get_list_prices(self, lenght, elements):
        prices = list(map(
            lambda x: float(self.client.get_symbol_ticker(symbol=x[0])['price']) * (float(
                self.client.get_symbol_ticker(symbol=(x[0][lenght:] + 'USDT'))['price']) if 'USDT' not in x[0] else 1),
            elements))
        return prices  # return list with prices(in $)

        # (float(self.client.get_symbol_ticker(symbol=(x[0][lenght:] + 'USDT'))['price']) if 'USDT' not in x else 1)

    def get_one_price(self, symbol):
        price = self.client.get_symbol_ticker(symbol=symbol)
        return float(price['price'])


main_class = CreateDeal()


def analyzer():
    arr = new_crypto_dict.crypto

    for i in arr:
        prs = main_class.get_list_prices(len(i), arr[i])  # list with prices in USDT

        quantity = round((110 / min(prs)), arr[i][prs.index(min(prs))][1])
        if ((quantity * max(prs)) - (quantity * min(prs))) > 1.5:  # change to 1 1.5 or 2
            symb_min = arr[i][prs.index(min(prs))]
            symb_max = arr[i][prs.index(max(prs))]
            if "USDT" not in symb_min[0]:
                print((quantity * max(prs)) - (quantity * min(prs)))
                print(symb_min)
                print(symb_max)
                symbol_usdt = symb_min[0][len(i):] + 'USDT'
                quantity_usdt = round((110 / main_class.get_one_price(symb_min[0][len(i):] + 'USDT')),
                                      rounds.round_usdt[symb_min[0][len(i):]])
                main_class.buy_market(symbol_usdt, quantity_usdt)
                main_class.buy_market(symb_min[0], quantity)
                main_class.sell_market(symb_max[0], quantity)
                main_class.sell_market(symbol_usdt, quantity_usdt)
                # print(arr[i][prs.index(min(prs))][len(i):] + 'USDT', quantity)
                # print(arr[i][prs.index(min(prs))], quantity)
                # print(arr[i][prs.index(max(prs))], quantity)
            else:
                print(arr[i][prs.index(min(prs))])
                print(arr[i][prs.index(max(prs))])
                print(((110 / min(prs)) * max(prs)) - 110)
                main_class.buy_market(arr[i][prs.index(min(prs))], quantity)
                main_class.sell_market(arr[i][prs.index(max(prs))], quantity)
                # print(arr[i][prs.index(min(prs))], q)
                # print(arr[i][prs.index(max(prs))], q)

    print(0)
