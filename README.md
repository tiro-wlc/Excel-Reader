# Excel-Reader
#主要读取特地excel文件特定行内容，默认是前10行，
#需要安装pandas和openxl包/模块
from __future__ import annotations
import argparse
import sys

try:
    import pandas as pd
except Exception:
    print("pandas is required. Install with: pip install -r requirements.txt")
    sys.exit(1)


def parse_args() -> argparse.Namespace:
    p = argparse.ArgumentParser(description="Print first N rows of an Excel file")
    p.add_argument("file", help="Path to the Excel file (.xlsx, .xls)")
    p.add_argument("--sheet", help="Sheet name or index (default: first sheet)")
    p.add_argument("--n", type=int, default=10, help="Number of rows to show (default: 10)")
    return p.parse_args()


def main() -> None:
    args = parse_args()
    sheet = args.sheet if args.sheet is not None else 0
    try:
        df = pd.read_excel(args.file, sheet_name=sheet, nrows=args.n)
    except Exception as e:
        print(f"Failed to read Excel file: {e}")
        sys.exit(2)

    if df.empty:
        print("在请求的范围内没有数据.")
        return

    # Print nicely
    print(df.head(args.n).to_string(index=False))


if __name__ == "__main__":
    main()
