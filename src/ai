#!/usr/bin/env python3

import requests
import sys
import time
import json
import argparse

AI_MODEL = "llama3.2:1b-instruct-q2_K"

def request_ai(prompt, model=AI_MODEL, host="http://127.0.0.1:11434"):
    url = f"{host}/v1/completions"
    headers = {
        "Content-Type": "application/json"
    }
    data = {
        "model": model,
        "keep_alive": -1,
        "prompt": prompt,
        "stream": True,
    }

    with requests.post(url, json=data, headers=headers, stream=True) as response:
        if response.status_code == 200:
            for line in response.iter_lines():
                if line:
                    json_response = json.loads(line.decode('utf-8').split('data: ')[1])
                    if 'choices' in json_response and len(json_response['choices']) > 0:
                        text = json_response['choices'][0]['text']
                        print(text, end='', flush=True)
                        if json_response['choices'][0]['finish_reason'] == 'stop':
                            print()
                            break
            return None
        else:
            print(f"Erreur {response.status_code}: {response.text}")
            return None

def parse_args():
    parser = argparse.ArgumentParser(description='AI Chat Interface')
    parser.add_argument('prompt', nargs='+', help='The prompt to send to the AI')
    parser.add_argument('--model', default=AI_MODEL, help='Model to use')
    parser.add_argument('--host', default='http://127.0.0.1:11434', help='Host URL')
    return parser.parse_args()

if __name__ == "__main__":
    args = parse_args()
    prompt = ' '.join(args.prompt)
    response = request_ai(prompt, model=args.model, host=args.host)
