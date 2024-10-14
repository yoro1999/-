<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>把握有几成</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Helvetica', 'Arial', sans-serif;
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            text-align: center;
        }
        input, button {
            font-size: 16px;
            padding: 10px;
            margin: 10px 0;
            width: 100%;
            box-sizing: border-box;
        }
        #result {
            margin-top: 20px;
            font-size: 18px;
            white-space: pre-wrap;
        }
    </style>
</head>
<body>
    <h1>把握有几成</h1>
    <input type="text" id="question" placeholder="请输入你的问题">
    <button onclick="divine()">求卦</button>
    <div id="result"></div>

    <script>
        const hexagrams = {
            1: ["乾卦", "大吉大利", 0.9],
            2: ["坤卦", "柔顺", 0.7],
            3: ["屯卦", "初始困难", 0.4],
            4: ["蒙卦", "启蒙", 0.5],
            5: ["需卦", "等待时机", 0.6],
            6: ["讼卦", "争执", 0.3],
            7: ["师卦", "领导才能", 0.7],
            8: ["比卦", "亲近", 0.8],
            9: ["小畜卦", "蓄势待发", 0.6],
            10: ["履卦", "谨慎行事", 0.7],
            11: ["泰卦", "通达", 0.9],
            12: ["否卦", "闭塞", 0.2],
            13: ["同人卦", "同道合作", 0.8],
            14: ["大有卦", "丰盛", 0.9],
            15: ["谦卦", "谦逊", 0.7],
            16: ["豫卦", "愉悦", 0.8],
            17: ["随卦", "跟随", 0.6],
            18: ["蛊卦", "振作", 0.5],
            19: ["临卦", "临近", 0.7],
            20: ["观卦", "观察", 0.6],
            21: ["噬嗑卦", "决断", 0.7],
            22: ["贲卦", "文饰", 0.6],
            23: ["剥卦", "剥落", 0.3],
            24: ["复卦", "复兴", 0.7],
            25: ["无妄卦", "无妄之灾", 0.4],
            26: ["大畜卦", "蓄养", 0.8],
            27: ["颐卦", "养育", 0.7],
            28: ["大过卦", "大过", 0.3],
            29: ["坎卦", "险难", 0.4],
            30: ["离卦", "光明", 0.8],
            31: ["咸卦", "感应", 0.7],
            32: ["恒卦", "恒久", 0.8],
            33: ["遁卦", "遁逃", 0.3],
            34: ["大壮卦", "大壮", 0.8],
            35: ["晋卦", "晋升", 0.8],
            36: ["明夷卦", "晦暗", 0.3],
            37: ["家人卦", "家庭", 0.7],
            38: ["睽卦", "乖离", 0.4],
            39: ["蹇卦", "艰难", 0.3],
            40: ["解卦", "解除", 0.7],
            41: ["损卦", "损减", 0.4],
            42: ["益卦", "增益", 0.8],
            43: ["夬卦", "决断", 0.7],
            44: ["姤卦", "遭遇", 0.5],
            45: ["萃卦", "聚集", 0.7],
            46: ["升卦", "上升", 0.8],
            47: ["困卦", "困境", 0.3],
            48: ["井卦", "循序渐进", 0.6],
            49: ["革卦", "革新", 0.7],
            50: ["鼎卦", "鼎新", 0.8],
            51: ["震卦", "震动", 0.5],
            52: ["艮卦", "止", 0.5],
            53: ["渐卦", "渐进", 0.7],
            54: ["归妹卦", "归宿", 0.6],
            55: ["丰卦", "丰富", 0.8],
            56: ["旅卦", "旅行", 0.6],
            57: ["巽卦", "顺从", 0.7],
            58: ["兑卦", "喜悦", 0.8],
            59: ["涣卦", "分散", 0.4],
            60: ["节卦", "节制", 0.7],
            61: ["中孚卦", "诚信", 0.8],
            62: ["小过卦", "小过", 0.5],
            63: ["既济卦", "成功在望", 0.8],
            64: ["未济卦", "尚未成功", 0.3]
        };

        function divine() {
            const question = document.getElementById('question').value.trim();
            if (question === '') {
                alert('请输入你的问题');
                return;
            }

            let result = localStorage.getItem(question);
            if (!result) {
                const seed = cyrb128(question);
                const rand = sfc32(seed[0], seed[1], seed[2], seed[3]);
                const hexagramNumber = Math.floor(rand() * 64) + 1;
                const [hexagramName, interpretation, probability] = hexagrams[hexagramNumber];
                result = `卦象：${hexagramName}\n把握有几成：${(probability * 10).toFixed(0)}成`;
                localStorage.setItem(question, result);
            }

            document.getElementById('result').textContent = result;
        }

        function cyrb128(str) {
            let h1 = 1779033703, h2 = 3144134277,
                h3 = 1013904242, h4 = 2773480762;
            for (let i = 0, k; i < str.length; i++) {
                k = str.charCodeAt(i);
                h1 = h2 ^ Math.imul(h1 ^ k, 597399067);
                h2 = h3 ^ Math.imul(h2 ^ k, 2869860233);
                h3 = h4 ^ Math.imul(h3 ^ k, 951274213);
                h4 = h1 ^ Math.imul(h4 ^ k, 2716044179);
            }
            h1 = Math.imul(h3 ^ (h1 >>> 18), 597399067);
            h2 = Math.imul(h4 ^ (h2 >>> 22), 2869860233);
            h3 = Math.imul(h1 ^ (h3 >>> 17), 951274213);
            h4 = Math.imul(h2 ^ (h4 >>> 19), 2716044179);
            return [(h1^h2^h3^h4)>>>0, (h2^h1)>>>0, (h3^h1)>>>0, (h4^h1)>>>0];
        }

        function sfc32(a, b, c, d) {
            return function() {
                a >>>= 0; b >>>= 0; c >>>= 0; d >>>= 0;
                var t = (a + b) | 0;
                a = b ^ b >>> 9;
                b = c + (c << 3) | 0;
                c = (c << 21 | c >>> 11);
                d = d + 1 | 0;
                t = t + d | 0;
                c = c + t | 0;
                return (t >>> 0) / 4294967296;
            }
        }
    </script>
</body>
</html># -
