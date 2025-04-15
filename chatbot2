import tkinter as tk
from tkinter import ttk, scrolledtext, messagebox
import json
import os
from datetime import datetime

class CustomButton(tk.Button):
    def __init__(self, parent, text, command, is_muted=False, **kwargs):
        bg_color = '#A5D6B7' if is_muted else '#19C37D'
        hover_color = '#81C784' if is_muted else '#15A36B'
        super().__init__(
            parent,
            text=text,
            command=command,
            bg=bg_color,
            fg='white',
            font=('Yu Gothic UI', 11, 'bold'),
            relief=tk.FLAT,
            padx=15,
            pady=8,
            cursor='hand2',
            justify='center',
            **kwargs
        )
        self.hover_color = hover_color
        self.normal_color = bg_color
        self.bind('<Enter>', self.on_enter)
        self.bind('<Leave>', self.on_leave)
        
    def on_enter(self, e):
        self['bg'] = self.hover_color
        
    def on_leave(self, e):
        self['bg'] = self.normal_color

class IconButton(tk.Button):
    def __init__(self, parent, command, **kwargs):
        super().__init__(
            parent,
            text='➤',
            command=command,
            bg='#19C37D',
            fg='white',
            font=('Yu Gothic UI', 16, 'bold'),
            relief=tk.FLAT,
            width=3,
            cursor='hand2',
            **kwargs
        )
        self.bind('<Enter>', self.on_enter)
        self.bind('<Leave>', self.on_leave)
        
    def on_enter(self, e):
        self['bg'] = '#15A36B'
        
    def on_leave(self, e):
        self['bg'] = '#19C37D'

class DeleteButton(tk.Button):
    def __init__(self, parent, text, command, **kwargs):
        super().__init__(
            parent,
            text=text,
            command=command,
            bg='#E53935',
            fg='white',
            font=('Yu Gothic UI', 10, 'bold'),
            relief=tk.FLAT,
            padx=10,
            pady=5,
            cursor='hand2',
            **kwargs
        )
        self.bind('<Enter>', self.on_enter)
        self.bind('<Leave>', self.on_leave)
        
    def on_enter(self, e):
        self['bg'] = '#C62828'
        
    def on_leave(self, e):
        self['bg'] = '#E53935'

class ChatbotGUI:
    def __init__(self, root):
        self.root = root
        self.knowledge_base = {
            "general_knowledge": {},
            "learned_knowledge": {},
            "conversation_history": []
        }
        self.load_knowledge()
        self.setup_gui()
        
    def load_knowledge(self):
        """知識ベースをファイルから読み込む"""
        try:
            if os.path.exists('knowledge_base.json'):
                with open('knowledge_base.json', 'r', encoding='utf-8') as f:
                    self.knowledge_base = json.load(f)
        except Exception as e:
            messagebox.showerror('エラー', f'知識ベースの読み込みエラー: {e}')
        
    def save_knowledge(self):
        """知識ベースをファイルに保存"""
        try:
            with open('knowledge_base.json', 'w', encoding='utf-8') as f:
                json.dump(self.knowledge_base, f, ensure_ascii=False, indent=2)
        except Exception as e:
            messagebox.showerror('エラー', f'知識ベースの保存エラー: {e}')
        
    def setup_gui(self):
        # ウィンドウ設定
        self.root.title('Learning Chatbot')
        self.root.geometry('1000x800')
        self.root.configure(bg='#343541')
        
        # スタイル設定
        style = ttk.Style()
        style.configure('Custom.TFrame', background='#343541')
        style.configure('Custom.TEntry', padding=8)
        
        # メインフレーム
        main_frame = ttk.Frame(self.root, style='Custom.TFrame')
        main_frame.pack(fill=tk.BOTH, expand=True)
        main_frame.grid_rowconfigure(0, weight=1)  # チャットエリアに重みを設定
        main_frame.grid_columnconfigure(0, weight=1)
        
        # チャットエリア
        self.chat_area = scrolledtext.ScrolledText(
            main_frame,
            wrap=tk.WORD,
            font=('Yu Gothic UI', 11),
            bg='#343541',
            fg='white',
            relief=tk.FLAT,
            padx=20,
            pady=10
        )
        self.chat_area.grid(row=0, column=0, sticky='nsew', padx=0, pady=0)
        self.chat_area.tag_configure('user', justify='left', spacing1=10, spacing3=10)
        self.chat_area.tag_configure('bot', justify='left', spacing1=10, spacing3=10)
        self.chat_area.tag_configure('user_bg', background='#343541')
        self.chat_area.tag_configure('bot_bg', background='#444654')
        
        # 入力エリアのコンテナ（固定高さ）
        input_container = ttk.Frame(main_frame, style='Custom.TFrame', height=120)
        input_container.grid(row=1, column=0, sticky='ew', padx=20, pady=(0, 20))
        input_container.grid_propagate(False)  # 高さを固定
        input_container.grid_columnconfigure(0, weight=1)
        
        # 入力エリアの中央配置用フレーム
        center_frame = ttk.Frame(input_container, style='Custom.TFrame')
        center_frame.grid(row=0, column=0, sticky='ew', pady=(20, 10))
        center_frame.grid_columnconfigure(0, weight=1)
        center_frame.grid_columnconfigure(1, weight=0)  # 送信ボタン用の列は固定幅
        
        # 入力エリア
        input_frame = ttk.Frame(center_frame, style='Custom.TFrame')
        input_frame.grid(row=0, column=0, sticky='ew', padx=(0, 10))
        input_frame.grid_columnconfigure(0, weight=1)
        
        # 入力フィールドの幅を制限（ウィンドウ幅の80%）
        self.input_field = ttk.Entry(
            input_frame,
            font=('Yu Gothic UI', 11),
            style='Custom.TEntry'
        )
        self.input_field.grid(row=0, column=0, sticky='ew', padx=(0, 10))
        self.input_field.bind('<Return>', lambda e: self.send_message())
        
        # 入力フィールドの幅を制限
        self.root.update_idletasks()
        window_width = self.root.winfo_width()
        input_width = int(window_width * 0.8)
        self.input_field.configure(width=int(input_width / 10))  # 文字数で幅を指定
        
        send_button = IconButton(
            center_frame,
            command=self.send_message
        )
        send_button.grid(row=0, column=1)
        
        # ボタンコンテナ
        button_container = ttk.Frame(input_container, style='Custom.TFrame')
        button_container.grid(row=1, column=0, sticky='w')
        
        # 学習データ追加ボタン（淡い色）
        add_knowledge_button = CustomButton(
            button_container,
            text='学習データを追加',
            command=self.show_add_knowledge_dialog,
            is_muted=True
        )
        add_knowledge_button.pack(side=tk.LEFT, padx=(0, 10))
        
        # 学習データ管理ボタン（淡い色）
        manage_knowledge_button = CustomButton(
            button_container,
            text='学習データを管理',
            command=self.show_manage_knowledge_dialog,
            is_muted=True
        )
        manage_knowledge_button.pack(side=tk.LEFT)
        
        # ウェルカムメッセージ
        self.add_message("こんにちは！学習型チャットボットです。何か質問や教えたいことがあればお気軽にどうぞ。", False)
        
    def add_message(self, text, is_user=True):
        self.chat_area.configure(state='normal')
        
        # メッセージの追加
        if is_user:
            self.chat_area.insert(tk.END, '\n', 'user_bg')
            self.chat_area.insert(tk.END, f'あなた: {text}\n', 'user')
        else:
            self.chat_area.insert(tk.END, '\n', 'bot_bg')
            self.chat_area.insert(tk.END, f'ボット: {text}\n', 'bot')
        
        # スクロールを最下部に
        self.chat_area.see(tk.END)
        self.chat_area.configure(state='disabled')
        
    def send_message(self):
        text = self.input_field.get().strip()
        if text:
            # ユーザーメッセージを表示
            self.add_message(text, True)
            
            # チャットボットの応答を生成
            response = self.generate_response(text)
            
            # ボットの応答を表示
            self.add_message(response, False)
            
            # 会話履歴に追加
            self.knowledge_base["conversation_history"].append({
                "timestamp": datetime.now().isoformat(),
                "user_input": text,
                "bot_response": response
            })
            
            # 知識ベースを保存
            self.save_knowledge()
            
            # 入力フィールドをクリア
            self.input_field.delete(0, tk.END)
            
    def generate_response(self, user_input: str) -> str:
        """応答の生成"""
        # 学習した知識から関連する情報を検索
        relevant_knowledge = self.search_knowledge(user_input)

        if relevant_knowledge:
            return f"学習した情報から: {relevant_knowledge}"
        else:
            return "申し訳ありません。その情報についてはまだ学習中です。"
            
    def search_knowledge(self, query: str) -> str:
        """知識ベースから関連する情報を検索"""
        # クエリに含まれる単語が知識ベースに存在するか確認
        for topic, info in self.knowledge_base["learned_knowledge"].items():
            if topic in query:
                return info["content"]
        return None
            
    def show_add_knowledge_dialog(self):
        # 学習データ追加ダイアログ
        dialog = tk.Toplevel(self.root)
        dialog.title('学習データを追加')
        dialog.geometry('500x300')
        dialog.configure(bg='#343541')
        
        # トピック入力
        topic_frame = ttk.Frame(dialog, style='Custom.TFrame')
        topic_frame.pack(fill=tk.X, padx=20, pady=10)
        
        ttk.Label(topic_frame, text='トピック:', foreground='white').pack(side=tk.LEFT)
        topic_entry = ttk.Entry(topic_frame, font=('Yu Gothic UI', 11))
        topic_entry.pack(side=tk.LEFT, fill=tk.X, expand=True, padx=(10, 0))
        
        # 内容入力
        content_frame = ttk.Frame(dialog, style='Custom.TFrame')
        content_frame.pack(fill=tk.BOTH, expand=True, padx=20, pady=10)
        
        ttk.Label(content_frame, text='内容:', foreground='white').pack(anchor=tk.W)
        content_text = scrolledtext.ScrolledText(
            content_frame,
            height=8,
            font=('Yu Gothic UI', 11),
            bg='#444654',
            fg='white'
        )
        content_text.pack(fill=tk.BOTH, expand=True, pady=(5, 0))
        
        def add_knowledge():
            topic = topic_entry.get().strip()
            content = content_text.get('1.0', tk.END).strip()
            
            if topic and content:
                # 新しい知識を追加
                self.knowledge_base["learned_knowledge"][topic] = {
                    "content": content,
                    "timestamp": datetime.now().isoformat(),
                    "confidence": "verified"
                }
                
                # 知識ベースを保存
                self.save_knowledge()
                
                messagebox.showinfo('成功', '学習データを追加しました。')
                dialog.destroy()
            else:
                messagebox.showerror('エラー', 'トピックと内容を入力してください。')
        
        # 追加ボタン
        button_frame = ttk.Frame(dialog, style='Custom.TFrame')
        button_frame.pack(fill=tk.X, padx=20, pady=10)
        
        CustomButton(
            button_frame,
            text='追加',
            command=add_knowledge
        ).pack(side=tk.RIGHT)
        
    def show_manage_knowledge_dialog(self):
        # 学習データ管理ダイアログ
        dialog = tk.Toplevel(self.root)
        dialog.title('学習データを管理')
        dialog.geometry('700x500')
        dialog.configure(bg='#343541')
        
        # タイトル
        title_frame = ttk.Frame(dialog, style='Custom.TFrame')
        title_frame.pack(fill=tk.X, padx=20, pady=10)
        
        ttk.Label(
            title_frame,
            text='学習データ一覧',
            font=('Yu Gothic UI', 14, 'bold'),
            foreground='white'
        ).pack(side=tk.LEFT)
        
        # データ一覧
        list_frame = ttk.Frame(dialog, style='Custom.TFrame')
        list_frame.pack(fill=tk.BOTH, expand=True, padx=20, pady=10)
        
        # リストボックス
        listbox = tk.Listbox(
            list_frame,
            font=('Yu Gothic UI', 11),
            bg='#444654',
            fg='white',
            selectbackground='#19C37D',
            selectforeground='white',
            relief=tk.FLAT,
            highlightthickness=0
        )
        listbox.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)
        
        # スクロールバー
        scrollbar = ttk.Scrollbar(list_frame, orient=tk.VERTICAL, command=listbox.yview)
        scrollbar.pack(side=tk.RIGHT, fill=tk.Y)
        listbox.config(yscrollcommand=scrollbar.set)
        
        # データをリストボックスに追加
        for topic in self.knowledge_base["learned_knowledge"].keys():
            listbox.insert(tk.END, topic)
        
        # 詳細表示エリア
        detail_frame = ttk.Frame(dialog, style='Custom.TFrame')
        detail_frame.pack(fill=tk.BOTH, expand=True, padx=20, pady=10)
        
        ttk.Label(
            detail_frame,
            text='詳細:',
            font=('Yu Gothic UI', 11, 'bold'),
            foreground='white'
        ).pack(anchor=tk.W)
        
        detail_text = scrolledtext.ScrolledText(
            detail_frame,
            height=5,
            font=('Yu Gothic UI', 11),
            bg='#444654',
            fg='white',
            relief=tk.FLAT
        )
        detail_text.pack(fill=tk.BOTH, expand=True, pady=(5, 0))
        
        # 選択された項目の詳細を表示
        def show_detail(event):
            selection = listbox.curselection()
            if selection:
                topic = listbox.get(selection[0])
                info = self.knowledge_base["learned_knowledge"][topic]
                detail_text.delete('1.0', tk.END)
                detail_text.insert('1.0', f"トピック: {topic}\n\n")
                detail_text.insert(tk.END, f"内容: {info['content']}\n\n")
                detail_text.insert(tk.END, f"追加日時: {info['timestamp']}")
        
        listbox.bind('<<ListboxSelect>>', show_detail)
        
        # ボタンフレーム
        button_frame = ttk.Frame(dialog, style='Custom.TFrame')
        button_frame.pack(fill=tk.X, padx=20, pady=10)
        
        def delete_selected():
            selection = listbox.curselection()
            if selection:
                topic = listbox.get(selection[0])
                if messagebox.askyesno('確認', f'「{topic}」を削除してもよろしいですか？'):
                    del self.knowledge_base["learned_knowledge"][topic]
                    self.save_knowledge()
                    listbox.delete(selection[0])
                    detail_text.delete('1.0', tk.END)
                    messagebox.showinfo('成功', '学習データを削除しました。')
            else:
                messagebox.showwarning('警告', '削除する項目を選択してください。')
        
        DeleteButton(
            button_frame,
            text='選択したデータを削除',
            command=delete_selected
        ).pack(side=tk.RIGHT)
        
        CustomButton(
            button_frame,
            text='閉じる',
            command=dialog.destroy
        ).pack(side=tk.RIGHT, padx=(0, 10))

def main():
    root = tk.Tk()
    app = ChatbotGUI(root)
    root.mainloop()

if __name__ == '__main__':
    main() 
