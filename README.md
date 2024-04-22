<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Coin Verileri</title>
    <style>
        body {
            font-family: Arial, sans-serif;
        }

        table {
            border-collapse: collapse;
            width: 100%;
            max-width: 1200px;
            margin: auto;
        }

        th, td {
            border: 1px solid black;
            padding: 8px;
            text-align: center;
        }

        th {
            background-color: #f2f2f2;
        }

        /* Responsive CSS */
        @media only screen and (max-width: 768px) {
            th, td {
                font-size: 12px;
                padding: 6px;
            }
        }

        .up-arrow {
            color: green;
        }

        .down-arrow {
            color: red;
        }

        .coin-rank {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            margin-right: 5px;
            display: inline-block;
            vertical-align: middle;
        }

        .coin-icon {
            width: 50px;
            height: 50px;
            vertical-align: middle;
        }

        /* Renkler */
        .positive-change {
            background-color: rgba(0, 255, 0, 0.1);
        }

        .negative-change {
            background-color: rgba(255, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <div style="overflow-x:auto;">
        <table>
            <tr>
                <th>Coin Simgesi</th>
                <th>Coin</th>
                <th>Fiyat</th>
                <th>ATH</th>
                <th>Yatırım Oranı</th>
                <th>1 Saatlik Değişim</th>
                <th>24 Saatlik Değişim</th>
                <th>1 Haftalık Değişim</th>
                <th>1 Aylık Değişim</th>
                <th>MA5 Destek</th>
                <th>RSI</th>
                <th>Direnç1</th>
                <th>Direnç2</th>
                <th>Direnç3</th>
                <th>24 Saatlik Ticaret Hacmi</th>
                <th>Piyasa Değeri</th>
            </tr>
            <?php
            // İlgili coinlerin sembollerini ve logolarını tanımlama
            $coins = array(
                'SHIB' => ['https://static.coinpaprika.com/coin/shib-shiba-inu/logo.png', '14,76%'],
                'FTM' => ['https://static.coinpaprika.com/coin/ftm-fantom/logo.png', '18,64%'],
                'ALGO' => ['https://static.coinpaprika.com/coin/algo-algorand/logo.png', '0,42%'],
                'RVN' => ['https://static.coinpaprika.com/coin/rvn-ravencoin/logo.png', '0,24%'],
                'OMAX' => ['https://static.coinpaprika.com/coin/omax-omax/logo.png', '0,57%'],
                'AREA' => ['https://static.coinpaprika.com/coin/area-areon-network/logo.png', '6,95%'],
                'CSPR' => ['https://static.coinpaprika.com/coin/cspr-casper-network/logo.png', '1,58%'],
                'BLOK' => ['https://static.coinpaprika.com/coin/blok-bloktopia/logo.png', '0,08%'],
                'LOVELY' => ['https://static.coinpaprika.com/coin/lovely-lovely-inu-finance/logo.png', '0,07%'],
                'OMC' => ['https://static.coinpaprika.com/coin/omc-omchain/logo.png', '0,50%'],
                'EXEN' => ['https://static.coinpaprika.com/coin/exen-exen/logo.png', '10,81%'],
                'BAD' => ['https://static.coinpaprika.com/coin/bad-bad-idea-ai/logo.png', '0,12%'],
                'XEP' => ['https://static.coinpaprika.com/coin/xep-electra-protocol/logo.png', '4,28%'],
                'XOR' => ['https://static.coinpaprika.com/coin/xor-sora/logo.png', '0,13%'],
                'KNINE' => ['https://static.coinpaprika.com/coin/knine-k9-finance-dao/logo.png', '0,03%'],
                'MET' => ['https://static.coinpaprika.com/coin/met-metchain/logo.png', '0,06%'],
                'BONE' => ['https://static.coinpaprika.com/coin/bone-bone-shibaswap/logo.png', '29,96%'],
                'HOT' => ['https://static.coinpaprika.com/coin/hot-holo/logo.png', '10,87%'],
                // Diğer coinler buraya eklenecek...
            );

            // Coinpaprika API'yi kullanmak için gerekli olan URL
            $url = 'https://api.coinpaprika.com/v1/tickers';

            // Veriyi çekmek için CURL kullanımı
            $curl = curl_init($url);
            curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
            $response = curl_exec($curl);
            curl_close($curl);

            // JSON verisini PHP dizisine dönüştürme
            $data = json_decode($response, true);

            // İstenilen coinlerin sembollerini tutacak dizi
            $displayedCoins = array();

            // İstenilen coinlerin verilerini döngüyle alma ve 24 Saatlik Değişim'e göre sıralama
            usort($data, function($a, $b) {
                return $b['quotes']['USD']['percent_change_24h'] - $a['quotes']['USD']['percent_change_24h'];
            });

            foreach ($data as $coin) {
                $coinSymbol = $coin['symbol'];
                if (isset($coins[$coinSymbol]) && !in_array($coinSymbol, $displayedCoins)) {
                    // Coin daha önce görüntülenmediyse
                    $displayedCoins[] = $coinSymbol; // Görüntülendi olarak işaretle


                    $coinPrice = number_format($coin['quotes']['USD']['price'], 8, '.', '');
                    $athPrice = isset($coin['quotes']['USD']['ath_price

']) ? number_format($coin['quotes']['USD']['ath_price'], 8, '.', '') : 'N/A'; // ATH fiyatını al
                    $coinChange24h = number_format($coin['quotes']['USD']['percent_change_24h'], 2, ',', '');
                    $coinChange1h = number_format($coin['quotes']['USD']['percent_change_1h'], 2, ',', '');
                    $coinChange1w = number_format($coin['quotes']['USD']['percent_change_7d'], 2, ',', ''); // 1 Haftalık Değişim
                    $coinChange1m = number_format($coin['quotes']['USD']['percent_change_30d'], 2, ',', ''); // 1 Aylık Değişim
                    $ma5Support = number_format($coinPrice * 0.95, 8, '.', ''); // MA5 Destek
                    $rsi = number_format(100 - (100 / (1 + ($coinPrice / $ma5Support))), 2, ',', '');
                    $resistance1 = number_format($coinPrice * 1.236, 8, '.', '');
                    $resistance2 = number_format($coinPrice * 1.382, 8, '.', '');
                    $resistance3 = number_format($coinPrice * 1.618, 8, '.', '');
                    $tradeVolume24h = number_format($coin['quotes']['USD']['volume_24h'], 2, '.', ','); // USD cinsinden ve binlik ayıraç
                    $marketCap = number_format($coin['quotes']['USD']['market_cap'], 2, '.', ',');

                    // 24 Saatlik Değişime göre sınıflandırma
                    $changeClass24h = $coinChange24h > 0 ? 'positive-change' : 'negative-change';

                    // Hedefe Kalan hesaplama
                    $remainingToATH = isset($coin['quotes']['USD']['ath_price']) ? number_format($athPrice / $coinPrice, 2, '.', '') : 'N/A';

                    // Tabloya verileri ekleme
                    echo "<tr class='$changeClass24h'>";
                    echo "<td><img src='" . $coins[$coinSymbol][0] . "' alt='$coinSymbol' class='coin-icon'></td>";
                    echo "<td>$coinSymbol</td>";
                    echo "<td>$coinPrice</td>";
                    echo "<td>$athPrice</td>"; // ATH fiyatı
                    echo "<td>" . $coins[$coinSymbol][1] . "</td>"; // Yatırım Oranı
                    echo "<td>$coinChange1h%</td>";
                    echo "<td>$coinChange24h%</td>";
                    echo "<td>$coinChange1w%</td>"; // 1 Haftalık Değişim
                    echo "<td>$coinChange1m%</td>"; // 1 Aylık Değişim
                    echo "<td>$ma5Support</td>";
                    echo "<td>$rsi</td>";
                    echo "<td>$resistance1</td>";
                    echo "<td>$resistance2</td>";
                    echo "<td>$resistance3</td>";
                    echo "<td>$tradeVolume24h</td>"; // 24 Saatlik Ticaret Hacmi
                    echo "<td>$marketCap</td>"; // Piyasa Değeri
                    echo "</tr>";
                }
            }
            ?>
        </table>
    </div>
</body>
</html>
