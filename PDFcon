import streamlit as st
import fitz  # PyMuPDF
from PIL import Image
import io

# 어플리케이션 제목
st.title("PDF to JPG Converter (without Poppler)")

# PDF 파일 업로드
uploaded_file = st.file_uploader("Choose a PDF file", type="pdf")

if uploaded_file is not None:
    # PDF 파일을 PyMuPDF로 열기
    pdf_document = fitz.open(stream=uploaded_file.read(), filetype="pdf")
    
    st.write(f"Total pages in the PDF: {pdf_document.page_count}")
    
    # PDF 페이지마다 이미지를 생성
    for page_num in range(pdf_document.page_count):
        page = pdf_document.load_page(page_num)
        pix = page.get_pixmap()  # 페이지를 이미지로 변환
        
        # 이미지 객체를 BytesIO로 저장 후 PIL 이미지로 변환
        img_data = io.BytesIO(pix.tobytes())
        img = Image.open(img_data)
        
        st.image(img, caption=f"Page {page_num + 1}", use_column_width=True)

        # 이미지 다운로드 링크 생성
        img_byte_arr = io.BytesIO()
        img.save(img_byte_arr, format="JPEG")
        img_byte_arr = img_byte_arr.getvalue()

        st.download_button(
            label=f"Download Page {page_num + 1} as JPG",
            data=img_byte_arr,
            file_name=f"page_{page_num + 1}.jpg",
            mime="image/jpeg"
        )
